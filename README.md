# Decoding Instagram User Analytics: From Data to Action

### Project Description

This project is aimed at deriving insightful data to significantly influence decisions based on data driven metrics for instagram, in order to grow and enhance the potency of the platform. The Point of view for this project is simple i.e., to use data from the instagram database and analyse the same to reach a potential resolution for some of the key tasks and assignments dedicated for the marketing team which boosts the ability of this platform among the users and in-turn generate and influence revenue based on data for the stakeholders of the platform.

###  Objectives
#### A. Marketing Analytics
- Help the marketing team and sought a solution for the team where it's required to reward the most loyal user.
- Encouraging inactive users to start posting by sending them promotional emails.
- The marketing team has organised a contest where the user with the most likes on a single photo wins.
- One of our partner brand wants to know the most popular hashtags to use in their posts to reach the most people
- Launch an ad campaign for which we need to figure out the best day to launch the same.
#### B. Investor Metrics:
- As the Investors want to know if users are still active and posting on Instagram or if they are making fewer posts.
- Provide details about fake and dummy accounts to the investors.

### Techstack Used
MySQL was chosen for this project as it closely aligns with real-world practices, where companies like Instagram retrieve data directly from databases for analysis. The objective of this project is to replicate or mimic authentic business scenarios, where data management plays a crucial role in decision-making. As a popular, open-source database management system, MySQL is renowned for its high performance, reliability. Additionally, its cost-effectiveness makes it an ideal choice for students and smaller projects, offering platform for efficiently managing and analyzing datasets.


### Approach/Steps followed 

- Loading data into MySQL, Instagram_dataset.docx consists of syntaxed prompt to fetch the data into the system(Simply run the same; This is done to streamline the process), by creating 'ig_clone' database. This prompt will create hundreds/thousands of rows along with multiple tables (comments, follows, likes, photo_tags, photos, tag and users) with different attributes each within them.

- To gain a clear understanding of the objectives outlined in this project, a file titled 'Business_Req_Docx_BRD_INSTAGRAM.docx' has been uploaded for reference.(This is a sample dataset and should be considered as such, given its smaller size).

- To offer a glimpse of this project, several key prompts/queries that align with the objectives have been outlined below. These prompts demonstrate how the goals of the project are being achieved.

- Help the marketing team and sought a solution for the team where it's required to reward the most loyal user.

        Select *, 
        dense_rank() over(order by created_at asc) as Loyal_user
        from users
        limit 5;

#### Output: 
![Image](https://github.com/user-attachments/assets/4997b82b-86b2-4661-8a1b-90d6e51bf136)


- The team has organized a contest where the user with the most likes on a single photo wins.
Your Task: Determine the winner of the contest and provide their details to the team.

        #from the likes table, we counted one user(count(user_id) has liked how many same photo_id using groupby(photo_id function; i.e., total count of user_id and group by photo id.
        #We get the total likes on each photo id

        With CTE_like as 
        (select photo_id, count(user_id) as Total_likes
        from likes
        group by photo_id),

        #Now from the photos table matached the data using inner join with column(id).
        #This gives us Total likes on each id and image url

        RESULTS AS(
        select id, Total_likes, image_url
        from CTE_like
        inner join photos
        on CTE_like.photo_id = photos.id)

        #Finally to filter the MAX(total likes) 

        SELECT id, Total_likes, image_url
        FROM RESULTS
        WHERE TOTAL_LIKES = (SELECT MAX(TOTAL_LIKES) FROM RESULTS);

#### Output: 
![Image](https://github.com/user-attachments/assets/f59e13cf-9dba-41a2-93fc-582440ece173)

- Launch an ad campaign for which we need to figure out the best day to launch the same.

        # We select the dayname from the timestamp and count the total users; 
        # We then group those total users by dayname(day of the week); we get total number of ids on each day of the week.

        SELECT DAYNAME(created_at) AS day_of_week, COUNT(id) as user_count 
        FROM users
        group by dayname(created_at) 
        order by user_count Desc 
        limit 1;

#### Output: 
![Image](https://github.com/user-attachments/assets/ca50cc57-3a0b-41d0-ab20-04e92c2fd77e)

- Investor Metrics: As the Investors want to know if users are still active and posting on Instagram or if they are making fewer posts.

        With cte_results As (
        select right_table.id, right_table.username,left_table.user_id, left_table.id as img_id, left_table.image_url
        From users right_table
        Left join photos left_table
        On right_table.id = left_table.user_id)

        Select round(count(image_url)/count(distinct user_id),1) as Avg_Post_Per_User, 
        round(count(image_url)/count(distinct id),1) as `Total_Posts/total_Users` 
        From cte_results;

#### Output:        
![Image](https://github.com/user-attachments/assets/fb8677c8-5f12-4d42-a31a-965fbba8075a)

  ### Insights/Outcome
- Key investor metrics have been extracted, revealing a minimal presence of fake users—only 13 unique IDs appear to be potentially bots out of a sample size of 100.(More details in 'MAIN_Instagram_User_Analytics_Report.pdf' file)
- Dataset containing 26 user IDs who have never posted on the platform. These users are identified as inactive contributors, having registered but not engaged in any posts or interactions within the platform's content
- In terms of engagement, 13 users have shown significant activity on Instagram, collectively making 257 comments each. This suggests that they are actively interacting with content, which could inform our marketing strategies moving forward.(More details in 'MAIN_Instagram_User_Analytics_Report.pdf' file)
- The top 5 most used hashtags—'smile,' 'beach,' 'party,' 'fun,' and 'food'—reflect a focus on positivity, social events, and leisure activities, highlighting users' engagement with happiness, relaxation, and community experiences.
- Moreover, our analysis indicates that Thursdays are the best day for launching marketing campaigns, as this day consistently attracts the highest number of registered users. Leveraging this insight could enhance our ability to convert leads into successful outcomes for our advertising efforts.(More details in 'MAIN_Instagram_User_Analytics_Report.pdf' file)

# P.S:
The above prompts provide a brief overview of the project and its objectives. For a comprehensive understanding of the entire output set, I have consolidated a detailed report, which has been uploaded in the file "MAIN_Instagram_User_Analytics_Report.pdf". This report includes a thorough summary of how the data was extracted, along with the results. Furthermore, it covers additional parameters extracted from the dataset, such as the most active users, highest commented photos, and more.
