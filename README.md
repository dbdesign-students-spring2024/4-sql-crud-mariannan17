# SQL Crud Report -- Marianna Nibu 
## Part 1: Restaurant finder
Disclaimer: For this assignment, in order to generate the restaurant names, I decided to use Mockaroo's "Plant Common Name" type in order to get 1000 different "restaurant names." For some reason, when trying to generate 1000 restaurant names in the "Custom List" type on Mockaroo, it wouldn't generate more than 100 different names. Knowing that my restaurant name record would have about 10 repetitions for each 1 restaurant name, I wanted my data to have more variety and have the least amount of repeitions, which is why I decided to go with a "plant-based theme" for the name of my restaurants, which seems to work out for some of the names. 

Once I started SQL from the command shell, I then created the first two tables with the relevant records needed for each table:
```sql
CREATE TABLE restaurants (
    restaurant_id INTEGER PRIMARY KEY,
    name TEXT,
    category TEXT,
    price_tier TEXT,
    neighborhood TEXT,
    opening_hours TEXT,
    average_rating REAL,
    good_for_kids INTEGER
);

CREATE TABLE reviews (
    review_id INTEGER PRIMARY KEY,
    restaurant_id INTEGER,
    first_name TEXT,
    last_name TEXT,
    description TEXT,
    rating TEXT,
    FOREIGN KEY (restaurant_id) REFERENCES restaurants(restaurant_id)
);
```
Then, I made sure to create a temporary restaurant table, since I knew I was going to have to import data from the CSV file. 
```sql
CREATE TABLE temp_restaurants (
    restaurant_id INTEGER,
    name TEXT,
    category TEXT,
    price_tier TEXT,
    neighborhood TEXT,
    opening_hours TEXT,
    average_rating TEXT,
    good_for_kids INTEGER
);
```
Here is the link to the practice CSV data file I used for this part of the assignment: [Click](data/restaurants.csv)

The SQL code that I used in order to import the practice CSV data files into the tables is:
```sql
.mode csv
.headers on
.import 4-sql-crud-mariannan17/data/restaurants.csv temp_restaurants --skip 1

INSERT INTO restaurants (restaurant_id, name, category, price_tier, neighborhood, opening_hours, average_rating, good_for_kids)
SELECT * FROM temp_restaurants;
```
Then in order to visualize the data I used these commands:
```sql
SELECT * FROM temp_restaurants;
SELECT * FROM restaurants;
```
I did this in order to make sure that first, the data was imported correctly and second, to make sure that once the data from the temp_restaurants table was imported into the restaurants table, all the data was formatted correctly and the way I wanted my data to look. 

## Restaurant: Queries
#### 1. Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
SQL Code Solution:
```sql
SELECT * FROM restaurants
WHERE neighborhood = 'Flushing' AND price_tier = 'Cheap';
```
The output:
```
restaurant_id,name,category,price_tier,neighborhood,opening_hours,average_rating,good_for_kids
92,"Kentucky Bluegrass","Saudi Arabian",Cheap,Flushing,0:35,"4 stars",false
105,"Papillose Wart Lichen",Azerbaijani,Cheap,Flushing,6:05,"3 stars",false
193,"Smooth Joyweed",Swiss,Cheap,Flushing,23:39,"1 star",true
291,"Hooded Coralroot",Moroccan,Cheap,Flushing,21:07,"5 stars",true
381,Mirrorplant,Belgian,Cheap,Flushing,22:18,"1 star",false
```
#### 2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
SQL Code solution:
```sql
SELECT * FROM restaurants
WHERE category = 'Brazilian' AND average_rating >= '3 stars'
ORDER BY average_rating DESC;
```
The output:
```
restaurant_id,name,category,price_tier,neighborhood,opening_hours,average_rating,good_for_kids
793,"Elegant Piperia",Brazilian,Cheap,"East Village",2:10,"5 stars",true
359,"Berlandier's Sundrops",Brazilian,Cheap,"Bulls Head",7:50,"4 stars",true
650,Tamujo,Brazilian,Medium,Eltingville,4:01,"4 stars",false
111,"Toadflax Penstemon",Brazilian,Expensive,Astoria,12:52,"3 stars",true
378,"Late Purple Aster",Brazilian,Expensive,Sunnyside,17:54,"3 stars",false
380,"Tuba Milkweed",Brazilian,Expensive,"Kew Gardens",20:07,"3 stars",false
736,"Sand Verbena",Brazilian,Cheap,"Little Italy",7:36,"3 stars",true
```

#### 3. Find all restaurants that are open now (see hint below).
SQL Code solution:
```sql
SELECT *
FROM restaurants
WHERE opening_hours <= strftime('%H:%M', 'now', 'localtime');
```
The output:
```
restaurant_id,name,category,price_tier,neighborhood,opening_hours,average_rating,good_for_kids
5,Lignumvitae,Hungarian,Medium,"Crown Heights",10:53,"2 stars",true
8,"Roundleaf Yellow Violet",Estonian,Cheap,"Crown Heights",13:41,"2 stars",false
9,"Japanese Raspberry",Jordanian,Expensive,"Morningside Heights",18:01,"2 stars",true
16,Sugarberry,Latvian,Expensive,"Mariners Harbor",14:00,"4 stars",true
20,Yampah,"Sri Lankan",Cheap,"Forest Hills",12:56,"1 star",false
21,"Burnham's Blackberry","South African",Expensive,"Marble Hill",12:39,"1 star",false
22,"Camasey Racimoso",Polish,Medium,"Port Morris",14:26,"5 stars",true
23,"Fir Clubmoss",Romanian,Cheap,"Financial District",13:00,"3 stars",true
30,"Bristly Nama",French,Expensive,Arrochar,12:57,"4 stars",true
32,"Creeping Juniper",Lithuanian,Medium,Douglaston,11:33,"4 stars",false
34,"Box Bedstraw",Iranian,Expensive,"Crown Heights",10:50,"1 star",true
38,"Chinese Wingnut",German,Expensive,Bushwick,0:49,"4 stars",false
43,Cliffbush,Bangladeshi,Medium,Hollis,17:16,"2 stars",false
44,"Southern Beech",Cypriot,Medium,"Long Island City",12:05,"5 stars",true
45,"Gurney's Hedgehog Cactus",Hawaiian,Expensive,Annadale,18:19,"2 stars",false
47,"Utah Agave",Indian,Cheap,Kingsbridge,16:46,"4 stars",true
50,Foolproofplant,Brazilian,Expensive,"Rockaway Beach",0:46,"1 star",false
53,"Old Man's Whiskers",Moroccan,Expensive,Willowbrook,15:20,"2 stars",true
54,"D'alton's Melaleuca",Mexican,Cheap,Tompkinsville,17:49,"3 stars",false
56,"Hedge Maple",Thai,Expensive,Rosebank,14:31,"1 star",true
57,"Common Dandelion",Swiss,Expensive,"Morningside Heights",17:17,"1 star",false
...
```
There were many more restaurants that were open, but for the sake of space, I decided to only put the first 20 restaurants from the output above.

#### 4. Leave a review for a restaurant (pick any restaurant as an example; note that leaving a review has no automatic effect on the average rating of the restaurant).
SQL Code solution:
```sql
INSERT INTO reviews (restaurant_id, first_name, last_name, description, rating)
VALUES (691, "Marianna", "Nibu", "Amazing food, would recommend Bigelow's Nolina!", " 5 stars");
SELECT * FROM reviews;
```
The output:
```
1|691|Marianna|Nibu|Amazing food, would recommend Bigelow's Nolina!| 5 stars
```

#### 5. Delete all restaurants that are not good for kids.
SQL Code solution:
```sql
DELETE FROM restaurants
WHERE good_for_kids = 'false';
SELECT * FROM restaurants;
```
The output:
```
1|Pipevine|Slovak|Cheap|Crown Heights|9:28|3 stars|true
2|Bonneville Shootingstar|Armenian|Cheap|Arrochar|19:36|4 stars|true
3|Wartleaf Ceanothus|Omani|Medium|Kips Bay|20:54|3 stars|true
4|Obovate Beakgrain|Georgian|Cheap|Boerum Hill|4:56|1 star|true
5|Lignumvitae|Hungarian|Medium|Crown Heights|10:53|2 stars|true
6|Chinese Tuliptree|Ethiopian|Medium|Clifton|8:48|5 stars|true
9|Japanese Raspberry|Jordanian|Expensive|Morningside Heights|18:01|2 stars|true
10|Low Spurge|Yemeni|Cheap|Grymes Hill|8:33|1 star|true
14|Gray's Milkvetch|Brazilian|Cheap|Boerum Hill|19:52|1 star|true
16|Sugarberry|Latvian|Expensive|Mariners Harbor|14:00|4 stars|true
19|Bigelow's Willow|Italian|Medium|Ridgewood|4:34|4 stars|true
22|Camasey Racimoso|Polish|Medium|Port Morris|14:26|5 stars|true
23|Fir Clubmoss|Romanian|Cheap|Financial District|13:00|3 stars|true
```
Only put the first few to save space. But they all have "true" instead of "false" now.

#### 6. Find the number of restaurants in each NYC neighborhood.
SQL Code solution:
```sql
SELECT neighborhood, COUNT(*) as num_restaurants
FROM restaurants
GROUP BY neighborhood;
```
The output:
```
Annadale|3
Arrochar|7
Astoria|5
Battery Park City|7
Bay Ridge|9
Bayside|3
Bed-Stuy|6
Bellerose|4
Boerum Hill|6
Briarwood|5
Bulls Head|9
Bushwick|3
Carroll Gardens|7
```
Only the first few to save space. 



## Part 2: Social media app
Disclaimer: I decided for the "visible" record I decided to use integers in order to make it easier to understand the difference between a post and a message. In this case, 0 means invisible and 1 means visible. 

Here are the first two SQL tables that I made: users and posts:

```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    username TEXT,
    email TEXT, 
    password TEXT
);

CREATE TABLE posts (
    post_id INTEGER PRIMARY KEY,
    user_id INTEGER,
    post_type TEXT,
    post_text TEXT,
    visible INTEGER,
    post_date TEXT,
    post_time TEXT,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```
Here is the link to the user.csv file [Click](data/users.csv)

Here is the link to the posts.csv file [Click](data/posts.csv)

Here is the code that I used to import the data from the users csv file into the users table: 
First I made a temp_users table

```sql
CREATE TABLE temp_users (
    user_id INTEGER,
    username TEXT,
    email TEXT,
    password TEXT
);

.mode csv
.headers on
.import 4-sql-crud-mariannan17/data/users.csv temp_users

INSERT INTO users (username, email, password)
SELECT username, email, password FROM temp_users;
```

Here is the code that I used to import the data from the posts csv file into the posts table: First I made a temp_posts table 

```sql
CREATE TABLE temp.temp_posts (
    post_id INTEGER,
    user_id INTEGER,
    post_type TEXT,
    post_text TEXT,
    visible INTEGER,
    post_date TEXT,
    post_time TEXT
);

.mode csv
.headers on
.import 4-sql-crud-mariannan17/data/posts.csv temp_posts

INSERT INTO posts (post_id, user_id, post_type, post_text, visible, post_date, post_time)
SELECT post_id, user_id, post_type, post_text, visible, post_date, post_time
FROM temp_posts;
```

## Social Media: Queries

#### 1. Register a new User.
SQL Code Solution:
```sql
INSERT INTO users (username, email, password) VALUES ('mariannan17', 'mariannan17@gmail.com', 'mariannaN123');
```
The output:
```
999,aheinsenrq,millsleyrq@examiner.com,iT4<+FK0
1000,mwebbrr,tsokalerr@i2i.jp,vA9()9z|N}|D.
1001,mariannan17,mariannan17@gmail.com,mariannaN123
```
#### 2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).
SQL Code Solution:
```sql
INSERT INTO posts (user_id, post_type, post_text, visible, post_date, post_time)
VALUES (1001, 'Message', 'Hi, how are you? I''m doing well!', 1, '2024-02-29', '12:00');
```
The output:
```
999,999,Story,"Believe you can and you're halfway there.",1,2024-01-02,12:33
1000,1000,Message,"stronger than you seem",1,2024-02-03,0:26
1001,1001,Message,"Hi, how are you? I'm doing well!",1,2024-02-29,12:00
```

#### 3. Create a new Story by a particular User (pick any User for example).
SQL Code Solution:
```sql
INSERT INTO posts (user_id, post_type, post_text, visible, post_date, post_time)
VALUES (996, 'Story', 'Hi everyone. Welcome to my story of the day!', 0, '2024-02-29', '16:15');
```

The output:
```
1000,1000,Message,"stronger than you seem",1,2024-02-03,0:26
1001,1001,Message,"Hi, how are you? I'm doing well!",1,2024-02-29,12:00
1002,996,Story,"Hi everyone. Welcome to my story of the day!",0,2024-02-29,16:15
```

#### 4. Show the 10 most recent visible Messages and Stories, in order of recency.
SQL Code Solution:
```sql
SELECT *
FROM posts
WHERE visible = 1
ORDER BY post_date DESC, post_time DESC
LIMIT 10;
```
The output:
```
post_id,user_id,post_type,post_text,visible,post_date,post_time
1001,1001,Message,"Hi, how are you? I'm doing well!",1,2024-02-29,12:00
849,849,Story,"but there's something good in every day.",1,2024-02-27,4:33
817,817,Story,"The best time to plant a tree was 20 years ago. The second best time is now.",1,2024-02-27,23:25
117,117,Story,"Lorem ipsum dolor sit amet",1,2024-02-26,8:11
632,632,Message,"failure is not fatal: It is the courage to continue that counts.",1,2024-02-26,6:06
820,820,Message,"The quick brown fox jumps over the lazy dog.",1,2024-02-26,2:25
938,938,Story,"Success is walking from failure to failure with no loss of enthusiasm.",1,2024-02-26,22:12
486,486,Story,"Every day may not be good",1,2024-02-26,1:28
22,22,Message,"The only person you should try to be better than is the person you were yesterday.",1,2024-02-26,17:08
236,236,Message,"Be yourself",1,2024-02-26,15:11
```
Only the first few to save space. 

#### 5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
SQL Code Solution:
```sql
SELECT *
FROM posts
WHERE user_id = 22           
  AND post_type = 'Message'
  AND visible = 1
  AND EXISTS (
    SELECT 1
    FROM posts AS p
    WHERE p.user_id = 22        
      AND p.post_id = posts.post_id
  )
ORDER BY post_date DESC, post_time DESC
LIMIT 10;
```
The output:
```
post_id,user_id,post_type,post_text,visible,post_date,post_time
22,22,Message,"The only person you should try to be better than is the person you were yesterday.",1,2024-02-26,17:08
```

#### 6. Make all Stories that are more than 24 hours old invisible.
There are no stories that were more than 24 hours old that were not already invisible. 
```sql
sqlite> SELECT *
FROM posts
WHERE post_type = 'Story'
AND visible = 1
AND JULIANDAY('now') - JULIANDAY(post_date || ' ' || post_time) * 24 > 24;
sqlite> 
```
There was no output since there was no records that met these requirements. This is something I could have improved on when creating my mock data from Mockaroo since i chose a date and time before this current date and time. 

#### 7. Show all invisible Messages and Stories, in order of recency.
SQL Code Solution
```sql
SELECT *
FROM posts
WHERE visible = 0
ORDER BY post_date DESC, post_time DESC;
```
The output:
```
ORDER BY post_date DESC, post_time DESC;
post_id,user_id,post_type,post_text,visible,post_date,post_time
1002,996,Story,"Hi everyone. Welcome to my story of the day!",0,2024-02-29,16:15
1003,996,Story,"Hi everyone. Welcome to my story of the day!",0,2024-02-29,16:15
304,304,Message,"everyone else is already taken.",0,2024-02-27,5:11
993,993,Message,"The future belongs to those who believe in the beauty of their dreams.",0,2024-02-27,4:07
507,507,Message,"smile because it happened.",0,2024-02-27,23:22
256,256,Story,"Lorem ipsum dolor sit amet",0,2024-02-27,22:47
391,391,Story,"but so are you.",0,2024-02-27,1:55
205,205,Message,"consectetur adipiscing elit.",0,2024-02-27,17:58
437,437,Message,"but learning to dance in the rain.",0,2024-02-27,12:29
```
Only the first few to save space. 

#### 8. Show the number of posts by each User.
SQL Code Solution
```sql
SELECT user_id, COUNT(*) AS num_posts
FROM posts
GROUP BY user_id;
```
The output
```
995,1
996,3
997,1
998,1
999,1
1000,1
1001,1
```
Only the last few to save space. 

#### 9. Show the post text and email address of all posts and the User who made them within the last 24 hours.
SQL Code Solution
```sql
SELECT p.post_text, u.email
FROM posts AS p
JOIN users AS u ON p.user_id = u.user_id
WHERE JULIANDAY('now') - JULIANDAY(p.post_date || ' ' || p.post_time) * 24 <= 24;
```
The output
```
post_text,email
"Life is not about waiting for the storm to pass",fmizen0@guardian.co.uk
"Life is 10% what happens to us and 90% how we react to it.",adomney1@go.com
"Life is 10% what happens to us and 90% how we react to it.",ssaill2@geocities.com
"Lorem ipsum dolor sit amet",pkneesha3@weather.com
"Happiness is a choice",gaish4@mysql.com
"stronger than you seem",hpeters8@addtoany.com
"In the end",hsarjant9@parallels.com
"Life is a journey",blembrickc@admin.ch
"Don't cry because it's over",awreakesf@who.int
```
Only the first few to save space. 

#### 10. Show the email addresses of all Users who have not posted anything yet.
There was no output since every email address has posting something. In order to get an output, I decided to see the email addresses of all users that have not posted a story yet.
```sql
SELECT email
FROM users
WHERE user_id NOT IN (
    SELECT DISTINCT user_id 
    FROM posts 
    WHERE post_type = 'Story'
);
```
The output
```
email
fmizen0@guardian.co.uk
gaish4@mysql.com
bwanjek7@ted.com
hpeters8@addtoany.com
hsarjant9@parallels.com
blembrickc@admin.ch
```
Only the first few to save space. 
