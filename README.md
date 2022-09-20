## Group By Queries

- Contare quanti iscritti ci sono stati per ogni anno

SELECT YEAR(`enrolment_date`) as 'year', COUNT(id) as 'students_by_year'
FROM `students`
GROUP BY YEAR(`enrolment_date`);
