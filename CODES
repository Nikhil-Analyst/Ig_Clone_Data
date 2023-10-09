# Ig_Clone_Data
This SQL data holds the details of small part of instagram Data. Tables related to Account, images, tags, etc is given in the data set. I have provided solutions with codes for some frequently asked questions. If you find it useful do rate my work.
Here are the codes with Questions.

# How many times does the average user post? hint -total no of photo/total no of user
SELECT round((SELECT COUNT(*)FROM photos)/(SELECT COUNT(*) 
FROM users),2) as avg__user_post;

# Find the top 5 most used hashtags.

select tag_name, count(*) as Top5_tags
from tags
inner join photo_tags on tags.id = photo_tags.tag_id 
group by tags.id
order by Top5_tags desc
limit 5;

# Find users who have liked every single photo on the site.

SELECT users.id,username, COUNT(users.id) As total_photos_liked
FROM users
JOIN likes ON users.id = likes.user_id
GROUP BY users.id
HAVING total_photos_liked = (SELECT COUNT(*) FROM photos);

# Retrieve a list of users along with their usernames and the rank of their account creation, ordered by the creation date in ascending order.

select id,created_at, username, rank() over (order by created_at asc) as Rank_acc
from users
order by created_at asc

# List the comments made on photos with their comment texts, photo URLs, and usernames of users who posted the comments. Include the comment count for each photo

select c.comment_text, p.image_url, u.username, count(c.comment_text) over (partition by p.image_url order by u.username) as Comment_count
from users u
inner join photos p on u.id = p.id
inner join comments c on p.user_id = c.user_id
group by c.comment_text, u.username, p.image_url
order by u.username, Comment_count desc

# For each tag, show the tag name and the number of photos associated with that tag. Rank the tags by the number of photos in descending order.

select t.tag_name, count(pt.photo_id) as photo_count, dense_rank() over (order by count(pt.photo_id) desc) as rank_photo
from tags t
inner join photo_tags pt on t.id=pt.photo_id
group by t.tag_name;

# List the usernames of users who have posted photos along with the count of photos they have posted. Rank them by the number of photos in descending order.

 select username, count(p.user_id) as photo_count, rank() over (order by count(user_id) desc) as rank_photo_count 
 from users u
 inner join photos p on u.id = p.user_id
 group by username

 # Display the username of each user along with the creation date of their first posted photo and the creation date of their next posted photo.

WITH Photo_rank AS (
    SELECT u.username, p.created_at AS current_photo_date,
	LEAD(p.created_at) OVER (PARTITION BY u.id ORDER BY p.created_at) AS next_photo_date,
	ROW_NUMBER() OVER (PARTITION BY u.id ORDER BY p.created_at) AS photo_rank
    FROM users u
    JOIN photos p ON u.id = p.user_id
)
SELECT username, current_photo_date, next_photo_date
FROM Photo_rank
WHERE photo_rank = 1;

# For each comment, show the comment text, the username of the commenter, and the comment text of the previous comment made on the same photo

with cte as (select
c.comment_text as current_comment,
u.username ,
lag(c.comment_text) over (partition by c.photo_id order by c.comment_text) as Previous_comment,
row_number() over (partition by c.photo_id order by c.comment_text) as comment_rank
from comments c 
inner join users u on c.id = u.id
)
select username, current_comment, Previous_comment
from cte;

# Show the username of each user along with the number of photos they have posted and the number of photos posted by the user before them and after them, based on the creation date.

with posted as (
select u.username, p.created_at, count(p.id) as Num_photos,
lag(count(p.id)) over (order by p.created_at) as Previous_photo,
lead (count(p.id)) over (order by p.created_at) as Next_photo
from users u
inner join photos p on u.id = p.user_id
group by u.id, u.username, p.created_at)
select username, created_at, Num_photos, Previous_photo, Next_photo
from posted order by created_at

THANK YOU
