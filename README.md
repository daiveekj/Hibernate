// hibernate.cfg.xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">


<<hibernate-configuration>
	<session-factory>
		<property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mphasisdb</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">root@39</property>
		<property name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>

		<property name="hibernate.hbm2ddl.auto">update</property>
		<property name="show_sql">true</property>

		<mapping resource="student.hbm.xml" />
		<mapping resource="course.hbm.xml" />

	</session-factory>
</hibernate-configuration>


// student.hcm.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping package="Model">

	<class name="Model.Student" table="student"> 

		<id name="studentId" column="student_id"> 
			<generator class="native" />
		</id>

		<property name="studentname" column="student_name" /> 
		<property name="course" column="course" />
		<property name="Mobile" column="mobile" />
		<property name="email" column="email" />

	</class>

</hibernate-mapping>


// App.java

public class App {
    public static void main(String[] args) {

        SessionFactory factory = new Configuration().configure().buildSessionFactory();

        // INSERT OPERATION
        Session insertSession = factory.openSession();
        Transaction tx1 = insertSession.beginTransaction();

        Student s = new Student(123, "john", "java", "243125", "john@gmail.com");
        insertSession.save(s);

        tx1.commit();
        insertSession.close();
        System.out.println(" INSERTED Successfully!\n");

        // FETCH ALL STUDENTS
        Session fetchSession = factory.openSession();

        List<Student> studentList = fetchSession
                .createQuery("from Student", Student.class)
                .list();

        System.out.println(" STUDENT RECORDS ");
        for (Student st : studentList) {
            System.out.println(st);
        }

        fetchSession.close();
        System.out.println(" FETCH Completed");

        // UPDATE STUDENT
        Session updateSession = factory.openSession();
        Transaction tx2 = updateSession.beginTransaction();

        Student stuToUpdate = updateSession.get(Student.class, 1);

        if (stuToUpdate != null) {
            stuToUpdate.setStudentname("JOHN s");
            stuToUpdate.setCourse("Advanced Java");
            stuToUpdate.setMobile("9876543210");
            stuToUpdate.setEmail("john@gmail.com");

            updateSession.update(stuToUpdate);
            System.out.println("UPDATED Successfully!");
        } else {
            System.out.println("Student not found for UPDATE");
        }

        tx2.commit();
        updateSession.close();
        System.out.println(" UPDATE Completed!\n");

        // DELETE
        Session deleteSession = factory.openSession();
        Transaction tx3 = deleteSession.beginTransaction();

        Student stuToDelete = deleteSession.get(Student.class, 1);

        if (stuToDelete != null) {
            deleteSession.delete(stuToDelete);
            System.out.println(" DELETED Successfully!");
        } else {
            System.out.println("Student not found for DELETE");
        }

        tx3.commit();
        deleteSession.close();
        System.out.println("DELETE Completed!\n");

        factory.close();
    }
}


