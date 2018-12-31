---
title: "Scraping Overflow"
date: 2018-12-31
header:
  teaser: /images/blogs/SO_logo.png
categories:
  - Blogs
tags: 
  - Pandas
  - SQL
  - R
  - Scraping
  - StackOverflow
  - API  
excerpt: "Making a dataset on Pandas question answered by 40 Gold users"
---

This works focuses upon creating a data set on **Pandas** Q/A over [StackOverflow](https://stackoverflow.com/tags/pandas/info).
Presently, there are more than [90k+ questions](https://stackoverflow.com/questions/tagged/pandas?sort=newest) available on StackOverflow which are asked under Pandas section. Many questions on SO have bad quality or are duplicate of already answered questions. A new SO user can ask a question which can fall in any of these sections. Similarly, a new SO user might not flag a question if it doesn't abide with SO guidelines. Therefore, users who have spent a long of efforts on SO are the ones who can classify a question as duplicated, can close them, downvote, etc.

We focus upon 40 such users who have earned [Pandas gold tag](https://stackoverflow.com/help/badges/3296/pandas) on their profile which in simple term means that they have answered enough questions to at least evaluate an upcoming question quality.

# Implementation

There is no need to perform any web scraping as such to extract SO data. SO provides an online API where one can simply run **SQL** query to get a downloadable CSV file. But before we start with the query, we need to fetch the SO Id of these 40 users. Their Id is present on their profile URL.
E.g.: https://stackoverflow.com/users/3877338/johne
Here, user *johne* has Id *3877338*

## Step 1
To do this I visited [this](https://stackoverflow.com/help/badges/3296/pandas) page and scrapped their feature which is *div.user-details a*.

## Step 2
Once I have the feature which gives me the profile link of all 40 users, I extracted these links using simple **R** script using *rvest* as follows -

-----------------------------------------------------
library(rvest)
library(stringr)
features <- c("div.user-details a")

  
main_site <- paste("https://stackoverflow.com/help/badges/3296/pandas")
main_site <- read_html(url(main_site,'rb'))
  
content <- data.frame(html_nodes(main_site,features) %>% html_attr("href"))

names(content) <- 'title'
content <- str_split_fixed(content$title, "/", 5)[,c(3, 4)]
write.csv(file = 'D:\\SO_users.csv',data.frame(content))
-----------------------------------------------------

## Step 3
Now I have a CSV file in which I stored the username and their IDs. I choose 10 Ids at a time, so making 4 sets of total 40 Ids and passed them to given SQL query on [Compose Query](https://data.stackexchange.com/stackoverflow/queries)

-----------------------------------------------------
-- TO DO:
-- exclude rolled-back edits from contributor list?
-- detect locked and migrated questions
-- make a whole-network version that returns posts from all SE sites


-- first find all posts by user (plus some other useful keys for joins)
create table #PostsByUser (
  PostId int primary key,
  ParentId int,
  PostTypeId int
)
insert into #PostsByUser
select
  Id as PostId, ParentId, PostTypeId
from
  Posts
where
OwnerUserId in (2411802, 2901002, 1427416, 653364, 704848, 487339, 190597, 644898, 776560, 1240268)
;

with
PostsByUser as (
  select * from #PostsByUser
),
-- find last body revision of each post (for Markdown content)
LastRevisions as (
  select
    PostsByUser.PostId as PostId,
    max(PostHistory.Id) as LastRevId
  from
    PostsByUser
    join PostHistory on PostHistory.PostId = PostsByUser.PostId
  where
    PostHistory.PostHistoryTypeId in (2,5,8)  -- create/edit/rollback body
  group by
    PostsByUser.PostId
),
-- find all editors for each post
Editors as (
  select
    PostHistory.PostId as PostId,
    PostHistory.UserId as UserId,
    coalesce(Users.DisplayName, PostHistory.UserDisplayName) as UserName,
    min(PostHistory.CreationDate) as FirstEditDate
  from
    PostsByUser
    join PostHistory on PostHistory.PostId = PostsByUser.PostId
    left join Users on Users.Id = PostHistory.UserId
  where
    PostHistory.PostHistoryTypeId in (4,5)  -- edit body/title
  group by
    PostHistory.PostId,
    PostHistory.UserId,
    coalesce(Users.DisplayName, PostHistory.UserDisplayName)
),
EditorsGrouped as (
  select
    PostId,
    string_agg(UserName + ' (' + coalesce(cast(UserId as nvarchar), 'n/a') + ')', ', ')
      within group ( order by FirstEditDate asc ) as EditorNames
  from
    Editors
  group by
    PostId
),
-- find tags for tag wiki posts and excerpts
WikiTags as (
  select
    PostId, TagName
  from
    PostsByUser
    join Tags on PostId = ExcerptPostId
  where
    PostTypeId = 4  -- tag excerpt
union
  select
    PostId, TagName
  from
    PostsByUser
    join Tags on PostId = WikiPostId
  where
    PostTypeId = 5  -- tag wiki
),
-- find duplicate questions
Dupes as (
  select distinct
    PostsByUser.PostId
  from
    PostsByUser
    join PostLinks on PostLinks.PostId = PostsByUser.PostId
  where
    PostTypeId = 1 and LinkTypeId = 3  -- question, duplicate
)

-- now select the actual result data
select
  Posts.Id as [Post Link],
  PostTypes.Name as [Type],
  coalesce(Posts.Title, Parent.Title) as [Title],  
  PostHistory.Text as [Markdown],
  coalesce(WikiTags.TagName, Posts.Tags, Parent.Tags) as [Tags],
  Posts.CreationDate as [Created],
  Posts.LastEditDate as [Last Edit],
  EditorNames as [Edited By],
  Posts.Score as [Score],
  Posts.FavoriteCount as [Favorites],
  coalesce(Posts.ViewCount, Parent.ViewCount) as [Views],
  Posts.AnswerCount as [Answers],
  iif(Posts.AcceptedAnswerId is not null or Posts.Id = Parent.AcceptedAnswerId, 'Accepted', null) as [Accepted],
  iif(Posts.CommunityOwnedDate is not null, 'CW', null) as [CW],
  iif(Posts.ClosedDate is not null, iif(Dupes.PostId is not null, 'Duplicate', 'Closed'), null) as [Closed]
from
  PostsByUser
  join Posts on Posts.Id = PostsByUser.PostId
  left join PostTypes on PostTypes.Id = PostsByUser.PostTypeId
  left join Posts as Parent on Parent.Id = PostsByUser.ParentId
  left join LastRevisions on LastRevisions.PostId = PostsByUser.PostId
  left join PostHistory on PostHistory.Id = LastRevisions.LastRevId
  left join EditorsGrouped on EditorsGrouped.PostId = PostsByUser.PostId
  left join WikiTags on WikiTags.PostId = PostsByUser.PostId
  left join Dupes on Dupes.PostId = PostsByUser.PostId
  
----------------------------------------------------- 

This resulted into 4 CSV files which is concatenated using given code in Python resulting into a final CSV file available here.
