1.create a user database and table for user details in MySQL:

create database userdb;

use userdb;

CREATE TABLE USER (
 USER_ID INT (5) NOT NULL,
 USERNAME VARCHAR (20) NOT NULL,
 CREATED_BY VARCHAR (20) NOT NULL,
 CREATED_DATE DATE NOT NULL,
 PRIMARY KEY ( USER_ID )
)

2. Add hibernate and mysql dependencies in POM.xml file

 <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.4.10.Final</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.18</version>
        </dependency>
 
    </dependencies>
3. Create a hibernate.cfg.xml file in src/main/resources

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
 
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">	jdbc:mysql://localhost:3306/userdb</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">root</property>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="show_sql">true</property>
        <property name="format_sql">true</property>
        <property name="hbm2ddl.auto">update </property>
        <mapping resource="user.hbm.xml" />
    </session-factory>
</hibernate-configuration>

4. Create a model POJO class
import java.util.Date;
 
public class User {
    private int userId;
    private String username;
    private String createdBy;
    private Date createdDate;
 
    public String getUsername() {
        return username;
    }
 
    public void setUsername(String username) {
        this.username = username;
    }
 
    public String getCreatedBy() {
        return createdBy;
    }
 
    public void setCreatedBy(String createdBy) {
        this.createdBy = createdBy;
    }
 
    public Date getCreatedDate() {
        return createdDate;
    }
 
    public void setCreatedDate(Date createdDate) {
        this.createdDate = createdDate;
    }
 
    public int getUserId() {
        return userId;
    }
 
    public void setUserId(int userId) {
        this.userId = userId;
    }
 
}

5. Create a mapping file user.hbm.xml  in src/main/resources 
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
"-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.net/hibernate-mapping-3.0.dtd">
 
<hibernate-mapping>
    <class name="com.javawebtutor.User"table="USER">
        <id name="userId"type="int"column="USER_ID">
            <generator class="assigned"/>
        </id>
        <property name="username">
            <column name="USERNAME"/>
        </property>
        <property name="createdBy">
            <column name="CREATED_BY"/>
        </property>
        <property name="createdDate"type="date">
            <column name="CREATED_DATE"/>
        </property>
    </class>
</hibernate-mapping>

6. Create HibernateUtil helper class
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
 
public class HibernateUtil {
    private static final SessionFactory sessionFactory = buildSessionFactory();
 
    private static SessionFactory buildSessionFactory() {
        try {
            // Create the SessionFactory from hibernate.cfg.xml
            return new Configuration().configure().buildSessionFactory();
        } catch (Throwable ex) {
            // Make sure you log the exception, as it might be swallowed
            System.err.println("SessionFactory creation failed." + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }
 
    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
 
    public static void shutdown() {
        // Close caches and connection pools
        getSessionFactory().close();
    }
 
}


7. Create a class to run the program

import java.util.Date;
 
import org.hibernate.Session;
 
public class Main {
    public static void main(String[] args) {
        Session session = HibernateUtil.getSessionFactory().openSession();
 
        session.beginTransaction();
        User user = new User();
 
        user.setUserId(1);
        user.setUsername("surya");
        user.setCreatedBy("Google");
        user.setCreatedDate(new Date());
 
        session.save(user);
        session.getTransaction().commit();
 
    }
 
}

