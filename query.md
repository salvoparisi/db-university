1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT * FROM `db-university`.students
WHERE YEAR(date_of_birth) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT * FROM `db-university`.courses
WHERE cfu > 10

3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT * FROM `db-university`.students
WHERE YEAR(CURDATE()) - YEAR(date_of_birth) >30

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

SELECT * FROM `db-university`.courses
WHERE period = "I semestre" AND year = 1

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

SELECT *FROM `db-university`.exams
WHERE date="2020-06-20" AND hour>"14:00:00"

6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT *FROM `db-university`.degrees
WHERE level="magistrale"

7. Da quanti dipartimenti è composta l'università? (12)

SELECT COUNT(*) FROM `db-university`.departments;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT COUNT(*) FROM `db-university`.teachers
WHERE phone IS NULL

9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo degree_id, inserire un valore casuale)

INSERT INTO `db-university`.students (degree_id, name, surname, date_of_birth, fiscal_code, enrolment_date, registration_number, email)
VALUES (42, 'Mario',  'Rossi', '1995-03-25', 'DJDSJI312DEF', '2019-02-21', '999999', 'mariorossi@gmail.com');

10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126

UPDATE `db-university`.teachers
SET office_number = 126
WHERE name="Pietro" AND surname="Rizzo";

11. Eliminare dalla tabella studenti il record creato precedentemente al punto 9

DELETE FROM `db-university`.students
WHERE name="Mario" && surname="Rossi"
