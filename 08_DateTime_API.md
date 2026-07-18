# 8. Date & Time API (java.time)

> Java 8 replaced the old `Date` and `Calendar` classes with a **new, modern, thread-safe** date-time API in the `java.time` package.

---

## 8.1 Problems with Old API

| Old API Problem | Java 8 Fix |
|----------------|------------|
| `Date` is mutable (not thread-safe) | All new classes are **immutable** |
| Months start from 0 (Jan = 0) | Months start from 1 (Jan = 1) |
| `SimpleDateFormat` is not thread-safe | `DateTimeFormatter` is thread-safe |
| No separate class for date-only or time-only | `LocalDate`, `LocalTime`, `LocalDateTime` |
| Timezone handling is confusing | `ZonedDateTime`, `ZoneId` |

---

## 8.2 Key Classes

| Class | What It Represents | Example |
|-------|-------------------|---------|
| `LocalDate` | Date only (no time) | `2026-07-18` |
| `LocalTime` | Time only (no date) | `14:30:00` |
| `LocalDateTime` | Date + Time (no timezone) | `2026-07-18T14:30:00` |
| `ZonedDateTime` | Date + Time + Timezone | `2026-07-18T14:30+05:30[Asia/Kolkata]` |
| `Instant` | Machine timestamp (epoch) | `2026-07-18T09:00:00Z` |
| `Duration` | Time-based amount | `2 hours, 30 minutes` |
| `Period` | Date-based amount | `1 year, 3 months, 5 days` |

---

## 8.3 LocalDate – Date Only

```java
import java.time.LocalDate;
import java.time.Month;

// Current date
LocalDate today = LocalDate.now();
System.out.println("Today: " + today); // 2026-07-18

// Specific date
LocalDate birthday = LocalDate.of(2000, Month.JANUARY, 15);
System.out.println("Birthday: " + birthday); // 2000-01-15

// From string
LocalDate parsed = LocalDate.parse("2026-12-25");
System.out.println("Christmas: " + parsed); // 2026-12-25

// Getting parts
System.out.println("Year:  " + today.getYear());        // 2026
System.out.println("Month: " + today.getMonth());       // JULY
System.out.println("Day:   " + today.getDayOfMonth());  // 18
System.out.println("DOW:   " + today.getDayOfWeek());   // SATURDAY

// Manipulating (returns NEW object – immutable!)
LocalDate tomorrow    = today.plusDays(1);
LocalDate nextMonth   = today.plusMonths(1);
LocalDate lastYear    = today.minusYears(1);
LocalDate nextWeek    = today.plusWeeks(1);

// Comparing
System.out.println(today.isBefore(tomorrow));  // true
System.out.println(today.isAfter(lastYear));   // true
System.out.println(today.isEqual(today));      // true
System.out.println(today.isLeapYear());        // false
```

---

## 8.4 LocalTime – Time Only

```java
import java.time.LocalTime;

// Current time
LocalTime now = LocalTime.now();
System.out.println("Now: " + now); // 14:30:45.123

// Specific time
LocalTime lunch = LocalTime.of(12, 30);
System.out.println("Lunch: " + lunch); // 12:30

LocalTime precise = LocalTime.of(14, 30, 45); // 14:30:45

// Getting parts
System.out.println("Hour:   " + now.getHour());
System.out.println("Minute: " + now.getMinute());
System.out.println("Second: " + now.getSecond());

// Manipulating
LocalTime later  = now.plusHours(2);
LocalTime earlier = now.minusMinutes(30);

// Comparing
System.out.println(lunch.isBefore(precise)); // true
```

---

## 8.5 LocalDateTime – Date + Time

```java
import java.time.LocalDateTime;

// Current date-time
LocalDateTime now = LocalDateTime.now();
System.out.println("Now: " + now); // 2026-07-18T14:30:45.123

// Specific date-time
LocalDateTime exam = LocalDateTime.of(2026, 8, 15, 10, 0);
System.out.println("Exam: " + exam); // 2026-08-15T10:00

// Combining date + time
LocalDate date = LocalDate.of(2026, 12, 25);
LocalTime time = LocalTime.of(9, 0);
LocalDateTime christmas = LocalDateTime.of(date, time);
System.out.println("Christmas morning: " + christmas);

// Extract date or time
LocalDate datePart = now.toLocalDate();
LocalTime timePart = now.toLocalTime();
```

---

## 8.6 ZonedDateTime – With Timezone

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;

// Current time in your timezone
ZonedDateTime now = ZonedDateTime.now();
System.out.println("Now: " + now);

// Specific timezone
ZonedDateTime indiaTime = ZonedDateTime.now(ZoneId.of("Asia/Kolkata"));
ZonedDateTime usTime    = ZonedDateTime.now(ZoneId.of("America/New_York"));

System.out.println("India:  " + indiaTime);
System.out.println("US:     " + usTime);

// List all available zones
Set<String> zones = ZoneId.getAvailableZoneIds();
zones.stream()
    .filter(z -> z.startsWith("Asia"))
    .sorted()
    .forEach(System.out::println);

// Convert between timezones
ZonedDateTime indiaToUS = indiaTime.withZoneSameInstant(ZoneId.of("America/New_York"));
System.out.println("India time in US: " + indiaToUS);
```

---

## 8.7 Instant – Machine Timestamp

```java
import java.time.Instant;

// Current timestamp (UTC)
Instant now = Instant.now();
System.out.println("Timestamp: " + now); // 2026-07-18T09:00:00.123Z

// Epoch seconds
long epochSecond = now.getEpochSecond();
System.out.println("Epoch: " + epochSecond);

// From epoch
Instant fromEpoch = Instant.ofEpochSecond(1000000000);
System.out.println(fromEpoch); // 2001-09-09T01:46:40Z

// Duration between instants
Instant start = Instant.now();
// ... some work ...
Instant end = Instant.now();
Duration elapsed = Duration.between(start, end);
System.out.println("Elapsed: " + elapsed.toMillis() + "ms");
```

---

## 8.8 Duration & Period

### Duration – Time-based (hours, minutes, seconds)
```java
import java.time.Duration;

Duration twoHours = Duration.ofHours(2);
Duration thirtyMin = Duration.ofMinutes(30);
Duration total = twoHours.plus(thirtyMin);

System.out.println(total);            // PT2H30M
System.out.println(total.toMinutes()); // 150

// Between two times
LocalTime start = LocalTime.of(9, 0);
LocalTime end   = LocalTime.of(17, 30);
Duration workDay = Duration.between(start, end);
System.out.println("Work day: " + workDay); // PT8H30M
```

### Period – Date-based (years, months, days)
```java
import java.time.Period;

Period oneYear = Period.ofYears(1);
Period threeMonths = Period.ofMonths(3);

// Between two dates
LocalDate start = LocalDate.of(2020, 1, 1);
LocalDate end   = LocalDate.of(2026, 7, 18);
Period between = Period.between(start, end);

System.out.println(between);             // P6Y6M17D
System.out.println(between.getYears());  // 6
System.out.println(between.getMonths()); // 6
System.out.println(between.getDays());   // 17
```

---

## 8.9 DateTimeFormatter – Formatting & Parsing

```java
import java.time.format.DateTimeFormatter;

LocalDateTime now = LocalDateTime.now();

// Built-in formatters
String iso = now.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);
System.out.println(iso); // 2026-07-18T14:30:45.123

// Custom patterns
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");
String formatted = now.format(formatter);
System.out.println(formatted); // 18/07/2026 14:30:45

DateTimeFormatter dateOnly = DateTimeFormatter.ofPattern("dd-MMM-yyyy");
String dateStr = now.format(dateOnly);
System.out.println(dateStr); // 18-Jul-2026

// Parsing a string to date
String input = "25/12/2026 09:00:00";
LocalDateTime parsed = LocalDateTime.parse(input, formatter);
System.out.println(parsed); // 2026-12-25T09:00

// Common patterns
// dd    → day (01-31)
// MM    → month number (01-12)
// MMM   → month short (Jan, Feb)
// MMMM  → month full (January)
// yyyy  → year (2026)
// HH    → hour 24h (00-23)
// hh    → hour 12h (01-12)
// mm    → minutes (00-59)
// ss    → seconds (00-59)
// a     → AM/PM
// E     → day of week (Mon)
// EEEE  → day of week full (Monday)
```

---

## 8.10 Converting Between Old and New API

```java
// java.util.Date → LocalDateTime
Date oldDate = new Date();
LocalDateTime newDate = oldDate.toInstant()
    .atZone(ZoneId.systemDefault())
    .toLocalDateTime();

// LocalDateTime → java.util.Date
LocalDateTime ldt = LocalDateTime.now();
Date old = Date.from(ldt.atZone(ZoneId.systemDefault()).toInstant());

// java.sql.Date → LocalDate
java.sql.Date sqlDate = java.sql.Date.valueOf("2026-07-18");
LocalDate localDate = sqlDate.toLocalDate();

// LocalDate → java.sql.Date
java.sql.Date back = java.sql.Date.valueOf(localDate);
```

---

## 8.11 Complete Example – Age Calculator

```java
import java.time.*;
import java.time.format.DateTimeFormatter;

public class AgeCalculator {
    public static void main(String[] args) {
        // Input
        String dobString = "15/01/2000";
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        LocalDate dob = LocalDate.parse(dobString, fmt);

        // Calculate age
        LocalDate today = LocalDate.now();
        Period age = Period.between(dob, today);

        System.out.println("Date of Birth: " + dob.format(fmt));
        System.out.println("Today:         " + today.format(fmt));
        System.out.println("Age:           " + age.getYears() + " years, "
                          + age.getMonths() + " months, "
                          + age.getDays() + " days");

        // Next birthday
        LocalDate nextBirthday = dob.withYear(today.getYear());
        if (nextBirthday.isBefore(today) || nextBirthday.isEqual(today)) {
            nextBirthday = nextBirthday.plusYears(1);
        }
        long daysUntil = java.time.temporal.ChronoUnit.DAYS.between(today, nextBirthday);
        System.out.println("Days until next birthday: " + daysUntil);
    }
}
```

---

## ✅ Quick Reference

| What You Need | Class to Use |
|--------------|-------------|
| Today's date | `LocalDate.now()` |
| Current time | `LocalTime.now()` |
| Date + time | `LocalDateTime.now()` |
| With timezone | `ZonedDateTime.now()` |
| Machine timestamp | `Instant.now()` |
| Time difference | `Duration.between()` |
| Date difference | `Period.between()` |
| Format/parse | `DateTimeFormatter` |

---

## ✅ Checklist

- [ ] I can create dates, times, and date-times
- [ ] I can manipulate dates (add/subtract days, months, years)
- [ ] I can format dates with custom patterns
- [ ] I can parse date strings into objects
- [ ] I understand Duration vs Period
- [ ] I can work with timezones

**Next → [09_Collectors.md](./09_Collectors.md)**
