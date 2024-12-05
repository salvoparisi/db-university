##Initial query

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

##GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

SELECT 
YEAR(enrolment_date) AS `year`,
COUNT(*) AS `number of students`
FROM `students`
GROUP BY YEAR(enrolment_date)

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT 
`office_address` AS `office_address`,
COUNT(*) AS `number of teachers`
FROM `teachers`
GROUP BY office_address

3. Calcolare la media dei voti di ogni appello d'esame

SELECT 
`exam_id` AS `exam_id`,
AVG(`vote`) AS `avarange_vote`
FROM `exam_student`
GROUP BY exam_id

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT 
`department_id` AS `department_id`,
COUNT(*) AS `numbers of degrees`
FROM `degrees`
GROUP BY department_id

##JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT *
FROM `students` 
JOIN `degrees` ON `degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT *
FROM `degrees` 
JOIN `departments` ON `department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' 
AND `level`='magistrale'

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT
`teachers`.`name` AS `teacher_name`,
`teachers`.`surname` AS `teacher_surname`,
`courses`.`name` AS `course_name`
FROM `teachers` 
JOIN `course_teacher`
JOIN `courses`
WHERE `course_teacher`.`teacher_id` = 44 AND `teachers`.`id` = 44 AND `course_teacher`.`course_id` = `courses`.`id`

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT
`students`.`name` AS `student_name`,
`students`.`surname` AS `student_surname`,
`degrees`.`name` AS `degrees_name`,
`departments`.`name` AS `department_name`
FROM `students`
JOIN `degrees`
JOIN `departments`
WHERE `students`.`degree_id`=`degrees`.`id` AND `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname` ASC

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT 
`degrees`.`name` AS `degree_name`,
`courses`.`name` AS `course_name`,
CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher_name`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
ORDER BY `degrees`.`name`, `courses`.`name`, `teachers`.`name`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

SELECT DISTINCT
CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`)
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica'
