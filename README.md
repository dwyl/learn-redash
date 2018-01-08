# Learn Redash
Learn how to use [Redash](https://redash.io/) to query and display your data in a dashboard.

## Why?
Technical team members can easily access a project's data writing SQL queries in the command line however for non-technical team members this is not the case. Having access to a project's data is valuable. It allows you to analyse and evaluate users' behaviours in order to make informed decisions about what the project needs going forward.

Dashboards are an appealing means of displaying a project's data in a user-friendly way. Whilst you can custom make a dashboard for your project this can be complex and time-consuming according to the needs of the client. Redash is a pre-made dashboard tool that is easy to connect to your data and offers a lot of inbuilt functionality to help solve these problems.

## What?

Redash is a paid for service that provides an online, up to date, queryable dashboard of a project's data. Prices start from $29 a month with a free 30 day trial, which is useful for those who may be using it as a stop-gap or want to test the waters of whether it will truly benefit them. To follow the instructions in this readme your project must be hosted on Heroku and using PostgreSQL.

## How?

### Create an account

To set up Redash on your project start by going to redash.io and entering in basic details to create an account (name, email, org name, password). You will be able to create accounts for other users at a later date but this number is capped on the free version, so it may be advisable to set up the account using a project email address if you have one.

### Add a data source

Having successfully created an account, next you need to connect your account to a 'data source' ie. your project! To connect your data source you will need to fill out the following page:

![image](https://user-images.githubusercontent.com/16775804/32285039-09508b56-bf21-11e7-9716-2961edc81ffb.png)

- If your page doesn't look like this to start, ensure your first option 'Type'
 has `PostgreSQL` selected.
- For the 'Name' value enter a name of your choice
- To fill in the remaining values you need to extract this data from your `DATABASE_URL` config variable found on Heroku (under 'Settings' , 'Reveal Config Vars'). The url is structured in the following way and you must dissect the values for each of these things:

postgres:// **user** : **password** @ **host** : **port** / **name**

Once you've entered your user/password/host/port and name values in you're ready to hit 'save' and your data source is now set up! You can click the 'Test Connection' button just to be sure everything's working ok.

### Querying your data source

Next to display your data to your client you should set up a query. Queries should be written in SQL to extract the information you desire. You should establish the technical proficiency and level of control a client wishes to have in order to decide what queries to write and who should be writing them. The results / data from all Redash queries can be downloaded into excel files so as a starting point you may wish to write a simple query to display the basic values of each of your tables and then less technical users can explore this data in excel which may be more familiar to them. Exploring data in downloaded files is also lower risk because if you change/delete data using your queries on Redash you will be editing your live data!

To set up a new query go to:
![image](https://user-images.githubusercontent.com/16775804/32285773-431b757e-bf23-11e7-93bf-66e64df68c1d.png)

And create a simple query such as `SELECT * FROM your-table-name;` to get all of the data from a given table:
![image](https://user-images.githubusercontent.com/16775804/32285945-d414d534-bf23-11e7-9941-5fea73df9803.png)
Conveniently, Redash provides you with the schema of your db so you can look up table and column names by exploring the box on the left hand side.

Once you've written your query click 'Execute' and then wait until the results of your query are displayed below:

![image](https://user-images.githubusercontent.com/16775804/32286104-4d6d2210-bf24-11e7-9708-76c0bcbcfdad.png)

In this example, as our query asked for the entire table, this is what our result shows. If you are happy with the query/results remember to click 'Save' so you can return to your query in the future! You can also see the option to download this content as a .csv or excel file on the right which is the option you may wish to select if choosing to share your data with your client in this way. If you want other team members to be able to see your query you should 'publish' it, then it will be visible to all users.

If you don't yet know SQL and want to know how to write more queries take a look at the following section:

### An introduction to SQL for writing queries

:bangbang: NOTE: Any commands that use **the words UPDATE, DROP, ALTER, CREATE, INSERT or DELETE will make permanent changes to your data**, do not use them unless you have checked with your team that you are not using your site's live data. :bangbang:

#### Select data from one table

A query is a request you make to your database for some information. The most simple query you can run is one which displays data for you. So if your database had a table called 'People' that looked like this:

**People**

| User ID | Name     | Age | Occupation      |
|---------|----------|-----|-----------------|
| 1       | Amy      | 29  | Violinist       |
| 2       | Bob      | 55  | Tree Surgeon    |
| 3       | Clarissa | 42  | Physiotherapist |

and you wanted to write a query that would display all of this data to you you would write:

`SELECT * FROM people;`

In this command you are selecting all of the data (the asterisk represents all values) from your table called 'People'. All SQL commands must end with a semi-colon to indicate that this is the end of your query.

#### Select data from two tables

In order to display data from two tables together you can do an inner join. So if I wanted to combine the values from the people table above with the pets table:

| pet_id | pet_name | animal | owner_id |
|--------|----------|--------|----------|
| 9      | Fido     | Dog    | 1        |
| 10     | Tiddles  | Cat    | 1        |
| 11     | Spot     | Dog    | 2        |

You would write:

```
SELECT * FROM people
JOIN pets
ON people.user_id = pets.owner_id;
```

An inner join will combine rows from different tables if the join condition is true. So joining the people and pets tables we would get a result like this:

| user_id | name | age | occupation   | pet_id | pet_name | animal | owner_id |
|---------|------|-----|--------------|--------|----------|--------|----------|
| 1       | Amy  | 29  | Violinist    | 9      | Fido     | Dog    | 1        |
| 1       | Amy  | 29  | Violinist    | 10     | Tiddles  | Cat    | 1        |
| 2       | Bob  | 55  | Tree Surgeon | 11     | Spot     | Dog    | 2        |

Notice how Amy appears twice because she has 2 pets whereas Clarissa no longer appears as she has no pets.

#### Select one column of data

If you just want to see one column of data run the following command:

```
SELECT people.name
FROM people;
```

Which will give you:

| Name |
|------|
| Amy  |
| Amy  |
| Bob  |

#### Further Learning
If you've got time to learn a bit more about SQL why not try:
- The Codeacademy tutorial - https://www.codecademy.com/learn/learn-sql
- Or this one from Mode - https://community.modeanalytics.com/sql/tutorial/introduction-to-sql/

Here's a **glossary of terms** for those more familiar with SQL looking for a reminder: https://www.codecademy.com/articles/sql-commands?r=master


### Creating accounts for multiple users

If your client would like to write their own queries then you can create an account for them to do so. It is possible to set up tiers of permissions so that you can restrict access in case certain users should be given read-only access. You can also use this restricted access to prevent certain users from seeing some parts of your dataset which may be confidential.
