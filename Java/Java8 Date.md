### Java8 Date

Java8 has new date and time classes to “replace” the old not-so-beloved java.util.Date class.

Unfortunately though, converting between the two is somewhat less obvious than you might expect.



##### **Convert java.util.Date to java.time.LocalDateTime**

```
Date ts = ...;
Instant instant = Instant.ofEpochMilli(ts.getTime());
LocalDateTime res = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
```

The big trick (for all these conversions) is to convert to Instant. This can be converted to LocalDateTime by telling the system which timezone to use. This needs to be the system default locale, otherwise the time will change.

##### **Convert java.util.Date to java.time.LocalDate**

```
Date date = ...;
Instant instant = Instant.ofEpochMilli(date.getTime());
LocalDate res = LocalDateTime.ofInstant(instant, ZoneId.systemDefault()).toLocalDate();
```

**Convert java.util.Date to java.time.LocalTime**

```
Date time = ...;
Instant instant = Instant.ofEpochMilli(time.getTime());
LocalTime res = LocalDateTime.ofInstant(instant, ZoneId.systemDefault()).toLocalTime();
```

##### **Convert java.time.LocalDateTime to java.util.Date**

```
LocalDateTime ldt = ...;
Instant instant = ldt.atZone(ZoneId.systemDefault()).toInstant();
Date res = Date.from(instant);
```

##### **Convert java.time.LocalDate to java.util.Date**

```
LocalDate ld = ...;Instant instant = ld.atStartOfDay().atZone(ZoneId.systemDefault()).toInstant();
Date res = Date.from(instant);
```

##### **Convert java.time.LocalTime to java.util.Date**

```
LocalTime lt = ...;
Instant instant = lt.atDate(LocalDate.of(A_YEAR, A_MONTH, A_DAY)).
        atZone(ZoneId.systemDefault()).toInstant();
Date time = Date.from(instant);
```



