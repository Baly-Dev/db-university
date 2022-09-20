\#\# Group By Queries
\- Contare quanti iscritti ci sono stati per ogni anno
\`\`\`sh
SELECT YEAR(\`enrolment\_date\`) as 'year', COUNT(id) as 'students\_by\_year'
FROM \`students\`
GROUP BY YEAR(\`enrolment\_date\`);
\`\`\`