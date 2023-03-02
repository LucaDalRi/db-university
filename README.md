# db-university


1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT `date_of_birth` FROM `students` WHERE `date_of_birth` LIKE '1990%'; 


2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT * FROM `courses` WHERE `cfu` > 10; 


3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT `date_of_birth` FROM `students` WHERE `date_of_birth` < '1993-28-02';    // Solo valido per la data di oggi 


4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)

SELECT `period` , `year` FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1; 


5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)

SELECT `date`, `hour` FROM `exams` WHERE `date` = '2020-06-20' AND `hour` > '14:00:00'; 


6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT `level` FROM `degrees` WHERE `level` = 'magistrale'; 


7. Da quanti dipartimenti è composta l'università? (12)

SELECT `id` FROM `departments`; 


8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT `phone` FROM `teachers` WHERE `phone` IS NULL; 









GROUP BY


1. Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(`id`) AS 'Numero_Iscritti', YEAR(`enrolment_date`) AS 'Anno_iscrizione' FROM `students` GROUP BY `Anno_iscrizione`; 


2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT COUNT(`id`) AS 'Nr_Insegnanti', `office_address` AS 'Locazione_Edificio' FROM `teachers` GROUP BY `Locazione_Edificio`; 


3. Calcolare la media dei voti di ogni appello d'esame

SELECT `exam_id` AVG(`vote`) AS 'Media_voti' FROM `exam_student` GROUP BY `exam_id`; 


4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT COUNT(`name`) AS 'Nr_Cdl', `department_id` AS 'Id_dipartimento' FROM `degrees` GROUP BY (`Id_dipartimento`); 






JOIN


1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT *
FROM `students`
INNER JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';


2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT * FROM `degrees` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` WHERE `departments`.`name` = 'Dipartimento di Neuroscienze'; 


3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT * FROM `course_teacher` WHERE `teacher_id` = 41; // un po' brutto 


4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT DISTINCT `students`.`name` , `students`.`surname`, `degrees`.`name`, degrees.level , `departments`.`name`
FROM `students`
INNER JOIN `degrees`
ON students.degree_id = degrees.id
INNER JOIN departments
ON degrees.id = departments.id
ORDER BY `students`.`surname` , `students`.`name`;


5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT DISTINCT `degrees`.`name`, `courses`.`name`, `teachers`.`name`, `teachers`.`surname` 
FROM `degrees`
INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `teachers`
ON `courses`.`id` = `teachers`.`id`;


6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)


7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per
superare ciascuno dei suoi esami