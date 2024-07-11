# LMS_EX

![b1](https://github.com/Carvingcode30/LMS_EX/assets/141935901/a746d856-1279-4ea5-9e6c-ff723160b462)



![a1](https://github.com/Carvingcode30/LMS_EX/assets/141935901/84456e7a-e345-4743-aa86-5a76d16722e4)


### SQL 쿼리 목록

```sql
-- Q1. '다교수'의 교수코드를 출력하세요
SELECT professor_code 
FROM professor 
WHERE professor_name = '다교수';

-- Q2. 학생번호, 학생이름, 키, 학과번호, 학과명 정보를 출력하세요
SELECT s.student_id, s.student_name, s.student_height, s.department_code, d.department_name 
FROM student s 
JOIN department d ON s.department_code = d.department_code;

-- Q3. 교수의 학과정보를 출력하세요.(교수이름, 학과명)
SELECT p.professor_name, d.department_name 
FROM professor p 
JOIN department d ON p.department_code = d.department_code;

-- Q4. 학과별 교수의 수를 출력하세요
SELECT d.department_name, COUNT(p.professor_code) 
FROM professor p 
JOIN department d ON p.department_code = d.department_code 
GROUP BY d.department_name;

-- Q5. 수학통계학의 학생정보를 출력하세요
SELECT d.department_name, s.student_id, s.student_name, s.student_height 
FROM student s 
JOIN department d ON s.department_code = d.department_code 
WHERE d.department_name = '수학통계학';

-- Q6. 학생 중 성이 '바'인 학생이 속한 학과이름과 학생이름을 출력하세요
SELECT d.department_name, s.student_name 
FROM student s 
JOIN department d ON s.department_code = d.department_code 
WHERE s.student_name LIKE '바%';

-- Q7. 학생의 키가 175~180 사이에 속하는 학생 수를 출력하세요
SELECT COUNT(s.student_id) AS count 
FROM student s 
WHERE s.student_height BETWEEN 175 AND 180;

-- Q8. 학과별 키의 최고값과 평균값을 출력하세요
SELECT d.department_name, MAX(s.student_height) AS max, AVG(s.student_height) AS avg 
FROM student s 
JOIN department d ON s.department_code = d.department_code 
GROUP BY d.department_name;

-- Q9. '다학생'과 같은 학과에 속한 학생의 이름을 출력하세요
SELECT student_name 
FROM student 
WHERE department_code = ( 
    SELECT department_code 
    FROM student 
    WHERE student_name = '다학생'
);

-- ★Q10. 가장 많은 학생이 있는 학과이름을 출력하세요
SELECT department_name 
FROM department 
WHERE department_code = ( 
    SELECT department_code 
    FROM student 
    GROUP BY department_code 
    HAVING COUNT(department_code) = ( 
        SELECT COUNT(department_code) cnt 
        FROM student 
        GROUP BY department_code 
        ORDER BY cnt DESC 
        LIMIT 1
    )
);

-- Q11. '교양 영어' 과목을 수강 신청한 학생의 이름을 출력하세요
SELECT s.student_name 
FROM student s 
WHERE student_id IN ( 
    SELECT student_id 
    FROM student_course 
    WHERE course_code = ( 
        SELECT course_code 
        FROM course 
        WHERE course_name = '교양 영어'
    )
);

-- Q12. '가교수' 교수의 과목을 수강신청한 학생수를 출력하세요
SELECT COUNT(student_id) 
FROM student_course 
WHERE course_code = ( 
    SELECT c.course_code 
    FROM professor p 
    JOIN course c ON p.professor_code = c.professor_code 
    WHERE p.professor_name = '가교수'
);

-- Q13. '사학생' 학생과 같은 과목을 수강신청한 학생이름을 출력하세요
SELECT s.student_name 
FROM student s 
JOIN student_course sc ON s.student_id = sc.student_id 
WHERE sc.course_code IN ( 
    SELECT course_code 
    FROM student_course 
    WHERE student_id = ( 
        SELECT student_id 
        FROM student 
        WHERE student_name = '사학생'
    )
);

-- Q14. '사학생'이 수강신청한 과목의 과목이름과 시작일자를 출력하세요
SELECT c.course_name, c.start_date 
FROM course c 
JOIN student_course sc ON c.course_code = sc.course_code 
JOIN student s ON sc.student_id = s.student_id 
WHERE s.student_name = '사학생';

-- Q15. 개설과목이름별 수강자 수를 출력하세요
SELECT c.course_name, COUNT(sc.student_id) AS cnt 
FROM student_course sc 
JOIN course c ON sc.course_code = c.course_code 
GROUP BY sc.course_code;


