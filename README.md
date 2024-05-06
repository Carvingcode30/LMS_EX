# LMS_EX

-- Q1. '다교수'의 교수코드를 출력하세요
select * from professor;

select professor_name, professor_code from professor
where professor_name = '다교수';

-- Q2. 학생번호, 학생이름, 키, 학과번호, 학과명 정보를 출력하세요

select 
s.student_id, s.student_name, s.student_height, s.department_code, d.department_name 
from student s
join department d
on s.department_code = d.department_code;

-- Q3. 교수의 학과정보를 출력하세요.(교수이름, 학과명)
select
p.professor_name, d.department_name
from professor p
join department d
on p.department_code = d.department_code;

-- Q4. 학과별 교수의 수를 출력하세요.
select * from professor;
select 
d.department_name, count(p.professor_code)
from professor p
join department d
on p.department_code = d.department_code
group by d.department_name;

-- Q5. 수학통계학의 학생정보를 출력하세요
select 
d.department_name, s.student_id, s.student_name, s.student_height
from student s
join department d
on s.department_code = d.department_code
where d.department_name = '수학통계학';

-- Q6. 학생 중 성이 '바'인 학생이 속한 학과이름과 학생이름을 출력하세요.
select 
d.department_name, s.student_name
from student s
join department d
on s.department_code = d.department_code
where s.student_name like '바%';

-- Q7. 학생의 키가 175~180 사이에 속하는 학생 수를 출력하세요.
select
count(s.student_id) as count
from student s
where s.student_height between 175 and 180;

-- Q8. 학과별 키의 최고값과 평균값을 출력하세요.
select 
d.department_name, max(s.student_height) as max, avg(s.student_height) as avg
from student s
join department d
on s.department_code = d.department_code
group by d.department_name;

-- Q9. '다학생'과 같은 학과에 속한 학생의 이름을 출력하세요.
select 
student_name
from student
where department_code=(
	select department_code from student where student_name='다학생'
);

-- ★Q10. 가장 많은 학생이 있는 학과이름을 출력하세요.
select * from student;
select * from department;

select 
count(department_code) cnt
from student
group by department_code
order by cnt desc limit 1;

select 
department_code
from student
group by department_code
having count(department_code) = 3;

select department_name
from department
where department_code = 1;

select
department_name
from department
where department_code=(
	select department_code
    from student
    group by department_code
    having count(department_code) = (
		select count(department_code) cnt
        from student
        group by department_code
        order by cnt desc limit 1
    )
);

-- Q11. '교양 영어' 과목을 수강 신청한 학생의 이름을 출력하세요.
select * from course;
select * from student_course;
select * from student;

select
c.course_name, s.student_name
from student s
join student_course sc
on s.student_id = sc.student_id
join course c
on sc.course_code = c.course_code
where sc.course_code = 1;

select student_name
from student
where student_id IN (
	select student_id
    from student_course
    where course_code= (
		select course_code
        from course
        where course_name='교양 영어'
   )
);

-- Q12. '가교수' 교수의 과목을 수강신청한 학생수를 출력하세요.
select * from professor;
select * from course;
select * from student_course;
select * from student;

select count(student_id)
from student_course
where course_code = (
	select c.course_code
    from professor p
    join course c
    on p.professor_code = c.professor_code
    where p.professor_name = '가교수'
);

-- Q13. '사학생' 학생과 같은 과목을 수강신청한 학생이름을 출력하세요.
select * from student;
select * from student_course;

select
c.course_name, s.student_name
from student s
join student_course sc
on s.student_id = sc.student_id
join course c
on sc.course_code = c.course_code
where c.course_code = 5;

-- Q14. '사학생'이 수강신청한 과목의 과목이름과 시작일자를 출력하세요.
select * from student;
select * from student_course;
select * from course;

select 
c.course_name, c.start_date
from course c
join student_course sc
on c.course_code = sc.course_code
join student s
on sc.student_id = s.student_id
where s.student_id = 7;

-- Q15. 개설과목이름별 수강자 수를 출력하세요.
select * from course;
select * from student_course;

select
c.course_name, count(student_id) as cnt
from student_course sc
join course c
on sc.course_code = c.course_code
group by sc.course_code;
