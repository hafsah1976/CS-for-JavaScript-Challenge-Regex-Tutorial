# MySQL REGEXP: Search Based On Regular Expressions

## Introduction

Regular expressions or RegEx are special string patterns that describe a search within a string (where a string can contain symbols, characters, numbers, words, and is treated as a text). It is a powerful, yet flexible tool used by developers to manipulate strings like search, extract, or validate data to get concise results. RegEx is commonly supported by almost all platforms, from programming languages to databases, including MySQL. A regular expression is generally not case sensitive and uses backslash as an escape character. A regular expression does not limit our search to a string based on a fixed pattern with (%) sign or an underscore (_) in the LIKE operator, we can use meta-characters that make regular expression flexible when matching patterns.

The term regular expression will be abbreviated as REGEXP from here onward in this tutorial.

## Summary

REGEXP allows us to search data matching in even more complex ways. REGEXP in MySQL is supported using REGEXP, which is used to match a pattern against a string. In MySQL, you can use RegEx in different ways, such as searching for data validation, particular patterns, and manipulating strings. REGEXP can be used in many ways in MySQL, including searching for specific patterns, validating data, and manipulating strings. Learning the concept of REGEXP in MySQL, your ability to work with text data in the databases will improve greatly. RegEx are used mostly in pattern matching in database queries. MySQL has built-in features and provides support for REGEXP to be used in queries. The REGEXP will always appear after the WHERE clause and will be used to filter records based on some matching criteria. An example, REGEXP can be formed to translate area codes of telephone numbers to geographical locations - this information can then perhaps be used to filter records by location.

In this tutorial, I'll explore how to use REGEXP to locate records in a MySQL database using e-mail addresses and phone numbers. I'll be focusing on some practical examples to demonstrate the usage of various regex components.

## Table of Contents

- [REGEXP Operators abd Functions in MySQL](#regexp-operators-and-functions-in-mysql)
- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Character Classes for Pattern Matching](#character-classes-for-pattern-matching)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Author](#author)

## REGEXP Operators and Functions in MySQL

If you're here looking for RegEx in MySQL, you must have some knowledge about the MySQL syntaxes. MySQL adapts the RegEx implementations to allow us to match patterns correctly  in SQL statements using the REGEX operator. You must follow a syntax that will let you use RegEx in MySQL. The basic syntax is

`SELECT columns FROM table WHERE field_name REGEXP 'pattern';`

...
columns:        The columns you want to retrieve.
table:          The name of the table you're querying.
field_name:     The field where you want to apply the REGEXP search.
regex_pattern:  The REGEXP pattern you want to use for matching.
...

This statement will return true if the value in the WHERE field_name matches the pattern. It will return false, if otherwise. If either the field_name or pattern is NULL, the result is always NULL. The negation form of the REGEXP operator is NOT REGEXP. Types of REGEXP Functions and Operators are:
_______________________________________________________________________________________________________________________________________________________________

Name:                Description
NOT REGEXP:          Negation of REGEXP
REGEXP:              Whether string matches regular expression
REGEXP_INSTR():      Starting index of substring matching regular expression
REGEXP_LIKE():       Whether string matches regular expression
REGEXP_REPLACE():    Replace substrings matching regular expression
REGEXP_SUBSTR():     Return substring matching regular expression
RLIKE:               Whether string matches regular expression
_______________________________________________________________________________________________________________________________________________________________

## Incorporating REGEXP Components

### Anchors

Now, let's explore how to incorporate various REGEXP components into your MySQL queries to make them even more powerful. Anchors, such as "^" (indicates start of line) and "$" (indicates end of line), can be used to specify where  REGEXP pattern should match in your string. For example, suppose you have a database table named contacts and you want to find email addresses at the beginning of a column, you can modify the query like this:

`SELECT * FROM contacts WHERE email_column REGEXP '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$';`

email_column: Replace this with the name of the column containing email addresses.

This query will return all records where the specified column matches the REGEXP pattern for valid email addresses.

### Quantifiers

Quantifiers in REGEXP are used to specify how many times a character or a group of characters should be matched. They control the repetition of characters or groups within a REGEXP pattern. Quantifiers like * (zero or more), + (one or more), and ? (zero or one) allow you to specify the number of times a character or group should be matched. For instance, to find records with repeated digits in a phone number, you can use:

`SELECT * FROM customers WHERE phone_column REGEXP '(\d)\1+';`

Here's an elaboration on the use of quantifiers:

- `SELECT * FROM customers`: To select all columns from the `customers` table.

- `WHERE phone_column REGEXP ...`: To filter the query, specify the condition for filtering records based on the content of the `phone_column`.

- `'(\d)\1+'`: This is the REGEXP pattern enclosed within single quotes `'...'`. Let's break down how it works:

  - `(\d)`: This part of the pattern is enclosed in parentheses `(...)` and captures a single digit. Here's what it does:
    - `\d`: This matches a digit (0-9).
  
  - `\1+`: This part is after the capturing group `(\d)` and specifies how many times the captured digit `\1` should be repeated.
    - `\1`: This is a back-reference to the first capturing group, which is the digit that was captured.
    - `+`: This is a quantifier that means "one or more times."

Here's how the pattern works:

1. It captures a single digit (0-9) using `(\d)`. This digit is stored for future reference in `\1`.

2. It then checks if the same digit is repeated one or more times immediately after the first captured digit. For example, if `phone_column` contains the number "1223334444," the regex pattern would match because it contains repeated digits (e.g., "22," "333," and "4444").

In summary, the REGEXP pattern `(\d)\1+` is looking for records where the `phone_column` contains repeated digits in the phone number. It captures a digit and then checks if that digit is repeated one or more times.

Quantifiers, such as `*` (zero or more), `+` (one or more), and `?` (zero or one), are powerful tools for specifying the repetition of characters or groups within regular expressions, allowing you to create flexible and precise matching patterns.

### Character Classes for Pattern Matching

Character classes, enclosed in square brackets `[ ]`, are a powerful feature of REGEXP that allow you to specify a set of characters you want to match at a particular position within a string. In your SQL query:

For instance, to find records containing specific characters in a content column:

`SELECT * FROM documents WHERE content_column REGEXP '[ABC]';`

- `SELECT * FROM documents`: To Selects all columns from the `documents` table.

- `WHERE content_column REGEXP ...`: To filter the query, specify the condition for filtering records based on the content of the `content_column`.

- `'[ABC]'`: This is the REGEXP pattern enclosed within square brackets `[ ]`. It defines a character class that specifies that you want to match any character that is one of the characters specified inside the square brackets. In this case, it matches either 'A', 'B', or 'C'.

For example, if `content_column` contains the text "The quick brown fox," the REGEXP pattern `[ABC]` would match the 'A' in "brown" because 'A' is one of the characters specified in the character class.

You can include more characters in the character class to match a broader set of characters. For example, `[AEIOUaeiou]` would match any vowel (both lowercase and uppercase).

Character classes are particularly useful when you want to find specific characters or sets of characters within a text column in a database, allowing you to create flexible and concise REGEXP patterns for pattern matching.

### Flags

Flags, such as ci for case-insensitive matching, can be added to your regex pattern. For example, to find email addresses regardless of case:

Suppose you have a database table named contacts, and you want to be able to find email addresses in a case-insensitive manner.

Here's the SQL query:

`SELECT * FROM contacts WHERE email_column REGEXP '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$' COLLATE utf8_general_ci;`

SELECT * FROM contacts: This part selects all columns from the contacts table.

WHERE email_column REGEXP ...: This is the filtering part of the query. It specifies the condition for filtering records.

'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$': This is the REGEXP pattern you want to use to match email addresses. It's the same pattern you used earlier to match email addresses. However, what makes it case-insensitive is the `COLLATE utf8_general_ci` part.

The magic happens at COLLATE clause utf8_general_ci in SQL that specifies the collation (sorting and comparison rules) for the comparison operation. In this example, utf8_general_ci stands for "UTF-8 (Unicode) with case-insensitive collation." It means that when comparing the email addresses in the email_column with the REGEXP pattern, it will be done in a case-insensitive manner.

So, with the COLLATE utf8_general_ci part added to your SQL query, the REGEXP pattern will match email addresses in the email_column, regardless of whether they are in uppercase or lowercase . This makes the query case-insensitive when searching for email addresses.

### Grouping and Capturing

For advanced queries, parentheses (...) are used for grouping in REGEXP patterns and can also capture parts of a matched pattern. For instance, to find records with repeating words:

`SELECT * FROM documents WHERE content_column REGEXP '(\b\w+\b)\s+\1';`

Let's break down the SQL query and regex pattern you provided:

In this query:

- `SELECT * FROM documents`: To select all columns from the `documents` table.

- `WHERE content_column REGEXP ...`: This is the filtering part of the query. It specifies the condition for filtering records based on the content of the `content_column`.

- `'(\b\w+\b)\s+\1'`: This is the REGEXP pattern enclosed in single quotes `'...'`. Now, let's break down the REGEXP pattern:

  - `(\b\w+\b)`: This part of the pattern is enclosed in parentheses `(...)`, which makes it a capturing group. What it does is:
    - `\b`: This is a word boundary anchor. It ensures that we match whole words, not partial words.
    - `\w+`: This matches one or more word characters (letters, digits, or underscores). It captures a word in the text.
    - `\b`: Another word boundary anchor, ensuring that we match the whole word.

  - `\s+`: This matches one or more whitespace characters (spaces, tabs, etc.).

  - `\1`: This is a back-reference to the first capturing group `(\b\w+\b)`. It ensures that the same word, which was captured by the first group, appears again immediately after one or more whitespace characters.

In essence, the REGEXP pattern `(\b\w+\b)\s+\1` is looking for repeated words in the `content_column`. Here's how it works:

1. The first part `(\b\w+\b)` captures a whole word.
2. The `\s+` matches one or more whitespace characters (e.g., space or tab).
3. The `\1` checks that the same word (captured in the first group) appears again immediately after the whitespace characters.

So, if you run this query, it will return records where the `content_column` contains repeated words separated by whitespace. For example, if `content_column` contains "the the quick brown fox," it would match because "the" is repeated.

This is just one example of how grouping and capturing in REGEXP patterns can be used for advanced queries to find specific patterns within text data in a database.

### Bracket Expressions

Bracket expressions, defined within [ ], allow you to create custom character sets. For example, to find records containing vowels:

`SELECT * FROM documents WHERE content_column REGEXP '[aeiouAEIOU]';`

A fundamental feature that allows you to specify a set of characters that you want to match in a particular position within a string. 
In the above SQL query:

- `SELECT * FROM documents`: select all columns from the `documents` table.

- `WHERE content_column REGEXP ...`: This is the filtering part of the query where the condition is specified for filtering records based on the content of the `content_column`.

- `'[aeiouAEIOU]'`: This is the REGEXP pattern enclosed within square brackets `[ ]`. It defines a character class or bracket expression. Let's break down how it works:
  - `[aeiouAEIOU]`: This character class specifies that you want to match any character that is one of the vowels (both lowercase and uppercase). In this case, `a`, `e`, `i`, `o`, `u`, `A`, `E`, `I`, `O`, or `U`.

Here's how the pattern works:

- It matches any character in the `content_column` that is one of the specified vowels (both lowercase and uppercase). For example, if `content_column` contains "The quick brown fox jumps over the lazy dog," the regex pattern would match all the vowels in the string, such as "e," "u," "i," "o," and "o."

So, the query effectively filters records where the `content_column` contains any of the specified vowels, either in lowercase or uppercase. It can be useful for tasks such as finding records that contain specific characters or sets of characters within a text column in a database.

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
