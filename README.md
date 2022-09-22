## GROUP BY Queries

### Contare quanti iscritti ci sono stati per ogni anno
```sh
SELECT YEAR(`enrolment_date`) as 'year', COUNT(id) as 'students_by_year'
FROM `students`
GROUP BY YEAR(`enrolment_date`);
```

### Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sh
SELECT `office_address` as 'office', COUNT(id) as 'n_teachers'
FROM `teachers`
GROUP BY `office_address`
```

### Calcolare la media dei voti di ogni appello d'esame

```sh
SELECT `exam_id` as 'exam', TRUNCATE(AVG(vote), 2) as 'votes_avarage' 
FROM `exam_student`
GROUP BY `exam_id`
```

### Contare quanti corsi di laurea ci sono per ogni dipartimento
```sh
SELECT `department_id` as 'department', COUNT(id) as 'degrees'
FROM `degrees`
GROUP BY `department_id`
```

## SELECT Queries

### Selzionare tutti gli studenti nati nel 1990 (160)
```sh
SELECT `name`, YEAR(`date_of_birth`) as 'year' 
FROM `students` 
WHERE YEAR(`date_of_birth`) = '1990';
```

### Selezionare tutti i corsi che valgono più di 10 crediti
```sh
SELECT  `name`, `cfu`
FROM `courses` 
WHERE `cfu`  > 10
```

### Selezionare gli studenti che hanno più di 30 anni
```sh
SELECT `name`, YEAR(`date_of_birth`) as 'date_of_birth'
FROM `students` 
WHERE YEAR(`date_of_birth`) < '1992'
```

### Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea
```sh
SELECT `name`, `description`, `period`, `year`
FROM `courses` 
WHERE `period` = 'I semestre' AND `year` = 1
```

### Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dope le 2:00pm) del 20/06/2020
```sh
SELECT `course_id`as 'course', `date`, `hour`
FROM `exams` 
WHERE `hour` > '14:00:00' AND `date` = '2020-06-20'
```

### Selezionare tutti i corsi di laurea magistrale
```sh
SELECT `name`, `level` 
FROM `degrees` 
WHERE `level` = 'magistrale'
```

### Da quanti dipartimenti è composta l'università?
```sh
SELECT COUNT(*) as `departments`
FROM `departments`
```

### Quanti sono gli insegnanti che hanno un numero di telefono?
```sh
SELECT `name`, `surname`, `phone`
FROM `teachers` 
WHERE phone IS NOT NULL
```

## JOIN Queries

### Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sh
SELECT 
`students`.`id`  AS 'student_id', 
`students`.`name` AS 'student_name', 
`students`.`surname` AS 'student_lastname', 
`degrees`.`name` AS 'degree_name'
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia'
```

### Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
```sh
SELECT 
`degrees`.`id` AS 'degree_id',
`degrees`.`name` AS 'degree_name',
`departments`.`name` AS 'deparment_name'
FROM `degrees`
JOIN `departments` 
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze'
```

### Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sh
SELECT 
`teachers`.`name` AS "teacher_name",
`teachers`.`surname` AS "teacher_lastname",
`courses`.`id` AS "course_id", 
`courses`.`name` AS "course_name"
FROM `courses`
JOIN `course_teacher`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44
```

### Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sh
SELECT 
`students`.`id` AS 'student_id',
`students`.`name` AS 'student_name',
`students`.`surname` AS 'student_lastname',
`degrees`.`name` AS 'degree_name',
`departments`.`name` AS 'department_name'
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`name` ASC, `students`.`surname` ASC
```

### Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sh
SELECT
`degrees`.`id` AS 'degree_id',
`degrees`.`name` AS 'degree_name',
`courses`.`name` AS 'course_name',
`teachers`.`name` AS 'teacher_name',
`teachers`.`surname` AS 'teacher_lastname'
FROM `courses`
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`
JOIN `course_teacher`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
```

### Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sh
SELECT DISTINCT
`teachers`.`id` AS 'teacher_id',
`teachers`.`name` AS 'teacher_name',
`teachers`.`surname` AS 'teacher_lastname',
`departments`.`name` AS 'department_name'
FROM `teachers`
JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica'
```

