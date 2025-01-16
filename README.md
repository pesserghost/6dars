-- 1-qism: Jadvallarni Loyihalash

-- books jadvali\CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    author_id INT NOT NULL,
    publisher_id INT NOT NULL,
    genre VARCHAR(100) NOT NULL,
    publish_date DATE NOT NULL,
    price DECIMAL(10, 2) NOT NULL CHECK (price > 0),
    CONSTRAINT fk_author FOREIGN KEY (author_id) REFERENCES authors(author_id) ON DELETE CASCADE,
    CONSTRAINT fk_publisher FOREIGN KEY (publisher_id) REFERENCES publishers(publisher_id) ON DELETE SET NULL
);

-- authors jadvali
CREATE TABLE authors (
    author_id SERIAL PRIMARY KEY,
    author_name VARCHAR(255) NOT NULL UNIQUE,
    birth_date DATE NOT NULL,
    country VARCHAR(100) NOT NULL
);

-- publishers jadvali
CREATE TABLE publishers (
    publisher_id SERIAL PRIMARY KEY,
    publisher_name VARCHAR(255) NOT NULL UNIQUE,
    city VARCHAR(100) NOT NULL,
    founded_year INT NOT NULL CHECK (founded_year > 0)
);

-- book_reviews jadvali
CREATE TABLE book_reviews (
    review_id SERIAL PRIMARY KEY,
    book_id INT NOT NULL,
    review_text TEXT,
    rating INT CHECK (rating BETWEEN 1 AND 10),
    review_date DATE DEFAULT CURRENT_DATE,
    CONSTRAINT fk_book FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE
);

-- 2-qism: Ma'lumotlarni Qo'shish

-- authors jadvaliga ma'lumot qo'shish
INSERT INTO authors (author_name, birth_date, country)
VALUES
    ('Islom Karimov', '1938-01-30', 'O\'zbekiston'),
    ('Abdulla Qodiriy', '1894-04-10', 'O\'zbekiston'),
    ('Chingiz Aytmatov', '1928-12-12', 'Qirg\'iziston'),
    ('Ernest Hemingway', '1899-07-21', 'AQSh'),
    ('Mark Twain', '1835-11-30', 'AQSh');

-- publishers jadvaliga ma'lumot qo'shish
INSERT INTO publishers (publisher_name, city, founded_year)
VALUES
    ('Nashriyot1', 'Toshkent', 1990),
    ('Nashriyot2', 'Samarqand', 1985),
    ('Nashriyot3', 'Buxoro', 2000),
    ('Nashriyot4', 'Andijon', 2015),
    ('Nashriyot5', 'Farg\'ona', 2010);

-- books jadvaliga ma'lumot qo'shish
INSERT INTO books (book_name, author_id, publisher_id, genre, publish_date, price)
VALUES
    ('O\'tgan kunlar', 2, 1, 'Roman', '1926-01-01', 45000),
    ('Ikki eshik orasi', 2, 2, 'Drama', '1935-01-01', 50000),
    ('Jamila', 3, 3, 'Hikoya', '1958-01-01', 30000),
    ('Chol va dengiz', 4, 4, 'Roman', '1952-01-01', 60000),
    ('Tom Soyer sarguzashtlari', 5, 5, 'Sarguzasht', '1876-01-01', 40000),
    ('Kitob1', 1, 1, 'Tarix', '2005-01-01', 55000),
    ('Kitob2', 1, 3, 'Ilmiy', '2010-01-01', 25000),
    ('Kitob3', 2, 2, 'Fantastika', '2015-01-01', 80000),
    ('Kitob4', 3, 4, 'She\'rlar', '2018-01-01', 20000),
    ('Kitob5', 4, 5, 'Detektiv', '2020-01-01', 35000);

-- book_reviews jadvaliga ma'lumot qo'shish
INSERT INTO book_reviews (book_id, review_text, rating)
VALUES
    (1, 'Juda ta\'sirli va ajoyib asar', 9),
    (3, 'Muallifning eng yaxshi hikoyasi', 8),
    (4, 'Hikoya sodda va o\'qish qiziqarli', 10),
    (5, 'Juda qiziqarli va ma\'noli', 9),
    (7, 'Ilmiy jihatdan juda foydali kitob', 7);

-- 3-qism: SQL So'rovlarni Bajarish

-- SELECT
SELECT * FROM books;
SELECT * FROM authors;
SELECT * FROM publishers;
SELECT * FROM book_reviews;

-- Column Aliases
SELECT book_name AS "Kitob nomi", price AS "Narxi" FROM books;

-- ORDER BY
SELECT * FROM authors ORDER BY author_name;
SELECT * FROM books ORDER BY price DESC;

-- WHERE
SELECT * FROM books WHERE genre = 'Roman';

-- LIMIT va FETCH
SELECT * FROM books LIMIT 5;

-- IN
SELECT * FROM books WHERE genre IN ('Roman', 'Hikoya');

-- BETWEEN
SELECT * FROM books WHERE price BETWEEN 30000 AND 60000;

-- LIKE
SELECT * FROM books WHERE book_name LIKE '%kitob%';

-- IS NULL
SELECT * FROM book_reviews WHERE review_text IS NULL;

-- 4-qism: JOIN qo'llash

-- JOIN
SELECT 
    b.book_name AS "Kitob nomi",
    a.author_name AS "Muallif",
    p.publisher_name AS "Nashriyot",
    b.genre AS "Janr",
    b.price AS "Narx"
FROM books b
JOIN authors a ON b.author_id = a.author_id
JOIN publishers p ON b.publisher_id = p.publisher_id;
