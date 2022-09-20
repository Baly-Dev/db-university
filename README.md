## Group By Queries

- Contare quanti iscritti ci sono stati per ogni anno
```sh
SELECT YEAR(`enrolment_date`) as 'year', COUNT(id) as 'students_by_year'
FROM `students`
GROUP BY YEAR(`enrolment_date`);
```

- Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sh
SELECT `office_address` as 'office', COUNT(id) as 'n_teachers'
FROM `teachers`
GROUP BY `office_address`
```

- Calcolare la media dei voti di ogni appello d'esame

```sh
SELECT `exam_id` as 'exam', TRUNCATE(AVG(vote), 2) as 'votes_avarage' 
FROM `exam_student`
GROUP BY `exam_id`
```

- Contare quanti corsi di laurea ci sono per ogni dipartimento
```sh
SELECT `department_id` as 'department', COUNT(id) as 'degrees'
FROM `degrees`
GROUP BY `department_id`
```