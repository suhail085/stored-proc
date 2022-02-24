# stored-proc
sql
Create database HRDB1;
use HRDB1
Create table Departments1(id int primary key identity,name varchar(max))
create table Employees1(id int primary key identity,name varchar(max),email varchar(max),
gender varchar(max),mobile varchar(20),
department_id int foreign key 
references Departments1 (id ))

--insert into Departments1 values(1,'suhail')
/*create  table employee(id int primary key identity,
name varchar(max),
email varchar(max),
gender varchar(20),
mobile varchar(30),
department_id int foreign key references Departments1(id)) */
--select * from Departments1;
--select * from Employees1;

go
create  proc sp_Departments1
@action varchar(20),
@id int=null,
@name varchar(100)
as 
begin 

if(@action='CREATE')
begin
insert into Departments(name) values(@name)
select 1 as result
end 
else if(@action='delete')
begin
delete from Departments where id=@id
select 1 as result
end
else if(@action='select')
begin 
select * from Departments
end 
else if(@action='update')
begin
update Departments set name=@name where id=@id
select 1 as result
end  
end
go

go
create proc sp_Employees1
@action varchar(20),
@id int=0,
@name varchar(100)=null,
@email varchar(100)=null,
@mobile varchar(10)=null,
@gender varchar(10)=null,
@depermant_id int =0
as begin
if(@action='create')
begin 
insert into Employees1(name,email,mobile,gender,department_id)
values(@name,@email,@mobile,@gender,@depermant_id)
select 1 result 
end
else if(@action='select')
begin
select * from Employees1
end
else if(@action='delete')
begin
delete  from Employees1 where id=@id
end
else if(@action='select_join')
begin
select e.Id,e.name,e.email,e.mobile,e.gender,d.Name 
from Employees1 e
full outer join 
Departments1 d
on e.department_id =d.id
end
else if (@action='update')
begin
update Employees1 set name=@name, email=@email, mobile=@mobile, 
@gender=@gender,department_id=@id where id=@id 
end
end


exec sp_Employees1 'select'
 exec sp_Employees1 'create',0,'rahul','rah@gmail.com','6756445','male',1
--exec sp_departments1 'create',0,'suhail'
--exec sp_departments1 'update',2,'HR department'
