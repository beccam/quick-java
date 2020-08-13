# DataStax Java Driver for Apache Cassandra™ Quickstart

[![Build Status](https://travis-ci.org/beccam/quickstart-java.svg?branch=master)](https://travis-ci.org/beccam/quickstart-java)

A basic CRUD Java application using the DataStax Java Driver for Apache Cassandra. Run the [GettingStartedComplete.java](/src/main/java/com/datastax/quickstart/GettingStartedComplete.java) file if you want to skip the exercise and run the application with the complete code.

Contributors: [Rebecca Mills](https://github.com/beccam)

## Objectives

* To demonstrate how to perform basic CRUD operations with the DataStax Java Driver.
* The intent is to help users get up and running quickly with the driver. 

## How this Sample Works
This project walks through basic CRUD operations using Cassandra. The demo application will first insert a row of user data, select that same row back out, update the row and finally delete the user. The README includes the code snippets to be filled in to the main application code to complete the functionality.

## Prerequisites
  * A running instance of [Apache Cassandra®](http://cassandra.apache.org/download/) 2.1+
  * [Maven](https://maven.apache.org/download.cgi) build automation tool
  * Java 8
  
## Project Layout

* [GettingStarted.java](/src/main/java/com/datastax/quickstart/GettingStarted.java) - Main application file with space to fill in CRUD operation code
* [users.cql](/src/main/resources/users.cql) - Use this file to create the schema  
  
## Create the keyspace and table
The `resources/users.cql` file provides the schema used for this project:

```sql
CREATE KEYSPACE demo
    WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'};

CREATE TABLE demo.users (
    lastname text PRIMARY KEY,
    age int,
    city text,
    email text,
    firstname text);
```

## Connect to your cluster

All of our code is contained in the `GettingStarted` class. 
Note how the main method creates a session to connect to our cluster and runs the CRUD operations against it. 
Replace the default parameters in `CqlSession.builder()` with your own hostname, port and datacenter.

```java
// TO DO: Fill in your own host, port, and data center
try (CqlSession session = CqlSession.builder()
                .addContactPoint(new InetSocketAddress("127.0.0.1", 9042))
                .withKeyspace("demo")
                .withLocalDatacenter("datacenter1")
                .build()) 
```

## CRUD Operations
Fill the code in the methods that will add a user, get a user, update a user and delete a user from the table with the driver.

### INSERT a user
```java
private static void setUser(CqlSession session, String lastname, int age, String city, String email, String firstname) {
    
    //TO DO: execute SimpleStatement that inserts one user into the table
    session.execute(
            SimpleStatement.builder( "INSERT INTO users (lastname, age, city, email, firstname) VALUES (?,?,?,?,?)")
            .addPositionalValues(lastname, age, city, email, firstname)
            .build());
}

```

### SELECT a user
```java
private static void getUser(CqlSession session, String lastname) {

    //TO DO: execute SimpleStatement that retrieves one user from the table
    //TO DO: print firstname and age of user
    ResultSet rs = session.execute(
    SimpleStatement.builder("SELECT * FROM users WHERE lastname=?")
            .addPositionalValue(lastname)
            .build());

    Row row = rs.one();
    System.out.format("%s %d\n", row.getString("firstname"), row.getInt("age"));
}
```

### UPDATE a user's age
```java
private static void updateUser(CqlSession session, int age, String lastname) {

    //TO DO: execute SimpleStatement that updates the age of one user
    session.execute(
            SimpleStatement.builder("UPDATE users SET age =?  WHERE lastname =? ")
            .addPositionalValues(age, lastname)
            .build());
}
```   

### DELETE a user
```java
private static void deleteUser(CqlSession session, String lastname) {

   //TO DO: execute SimpleStatement that deletes one user from the table
    session.execute(
            SimpleStatement.builder("DELETE FROM users WHERE lastname=?")
                    .addPositionalValue(lastname)
                    .build());

}
```
    


