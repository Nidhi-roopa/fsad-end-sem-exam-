package com.klef.fsad.exam;

import javax.persistence.*;
import java.util.Date;

@Entity
public class Payment {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String status;
    private double amount;

    @Temporal(TemporalType.DATE)
    private Date date;

    public Payment() {}

    public Payment(String name, Date date, String status, double amount) {
        this.name = name;
        this.date = date;
        this.status = status;
        this.amount = amount;
    }
}

package com.klef.fsad.exam;

import org.hibernate.*;
import org.hibernate.cfg.Configuration;
import org.hibernate.query.Query;
import java.util.Date;

public class ClientDemo {
    public static void main(String[] args) {

        Session session = new Configuration().configure()
                          .buildSessionFactory().openSession();

        // INSERT
        Transaction t1 = session.beginTransaction();
        Payment p = new Payment("Nidhi", new Date(), "SUCCESS", 5000);
        session.save(p);
        t1.commit();

        // DELETE using HQL
        Transaction t2 = session.beginTransaction();
        Query q = session.createQuery("delete from Payment where id=:pid");
        q.setParameter("pid", 1);
        int res = q.executeUpdate();
        t2.commit();

        System.out.println("Deleted: " + res);

        session.close();
    }
}
