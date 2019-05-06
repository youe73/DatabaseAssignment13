# DatabaseAssignment13

Due to the same complication error as the previous assignment 12, there is no way Docker can share my files.

![image](https://user-images.githubusercontent.com/40825848/57201875-007a2980-6f9f-11e9-9c75-32cd93ffb404.png)

It means I could not run the thing on my computer. 
    docker: Error response from daemon: Drive sharing failed for an unknown reason.
    See 'docker run --help'.

The cypher query to load the data is:

    USING PERIODIC COMMIT
    LOAD CSV WITH HEADERS FROM "file:///social_network_nodes.csv" AS row  FIELDTERMINATOR ","
    WITH row
    CREATE (a:person {
        pid: toInteger(row.node_id),
      name: row.name,
      job: row.job,
      birthday: row.birthday
    })

For mysql it is the syntax stated below

    use socialnetwork;

    CREATE TABLE socialnetwork_node (
      node_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
      name VARCHAR(100) CHARACTER SET latin1 DEFAULT NULL,
      job VARCHAR(100),
      birthday date
    );

    CREATE TABLE socialnetwork_edge (
      source_node_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
      target_node_id Int
    );

    SET GLOBAL local_infile = 1;
    LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/social_network_nodes.csv' INTO TABLE socialnetwork_node FIELDS TERMINATED BY ','  ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
    LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/social_network_edges.csv' INTO TABLE socialnetwork_edge FIELDS TERMINATED BY ','  ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;


Neo4j graph database

To create the relation

USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "file:///social_network_edges.csv" AS row  FIELDTERMINATOR ","
WITH row
Match (p1:person {pid: toInteger(row.source_node_id)}), (p2:person {pid: toInteger(row.target_node_id) })
create (p1)-[:Endorsments]->(p2)

The mysql query is

select count(persons.name) from nodes
left join edges on nodes.node_id = edges.source_node_id
left join nodes as persons on edges.target_node_id= persons.node_id
where nodes.node_id = 0

Adding primary key to tables in order to create the relation and to add index
    ALTER TABLE `social`.`nodes`
    CHANGE COLUMN `node_id` `node_id` INT(11) NOT NULL ,
    ADD PRIMARY KEY (`node_id`);

    ALTER TABLE `social`.`edges`
    ADD INDEX `inx_src` (`source_node_id` ASC),
    ADD INDEX `inx_trg` (`target_node_id` ASC);
    
 To select the dept in mysql
 
     select count(distinct persons2.node_id) from nodes
    left join edges as endorsments1 on nodes.node_id = endorsments1.source_node_id
    left join nodes as persons1 on endorsments1.target_node_id= persons1.node_id
    left join edges as endorsments2 on persons1.node_id = endorsments2.source_node_id
    left join nodes as persons2 on endorsments2.target_node_id= persons2.node_id
    where nodes.node_id = 0

 The comparison could not be made between the databases 



