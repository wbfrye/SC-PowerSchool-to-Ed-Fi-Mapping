# SC-PowerSchool-to-Ed-Fi-Mapping

classPeriods	classPeriodName			SC		Composite based on school_number, period and cycle day	FORMAT:  sch.school_number '-' coalesce( per.abbreviation, per.period_number ) '-' cy.letter
classPeriods	schoolId			Y	SCHOOLID	State-assigned value based on PERIOD.schoolid	
classPeriods	meetingTimes	startTime		Y	START_TIME	start_time	"( 	select 	distinct to_char(to_date(bsi.end_time, 'sssss'),'hh24:mi:ss') as end_time, 
		to_char(to_date(bsi.start_time, 'sssss'),'hh24:mi:ss') as start_time , 
		per.dcid as period_dcid , cy.dcid as cycle_dcid , bsi.DAILY_ATTENDANCE_CODE as official_attendanceperiod
	from	period per
	join 	schools sch on per.schoolid = sch.school_number
	join 	cycle_day cy on cy.schoolid = per.schoolid and cy.year_id = per.year_id
	join 	BELL_SCHEDULE_ITEMS bsi on 	bsi.PERIOD_ID=per.id AND sch.state_excludefromreporting = 0 
										and per.period_number &gt; 0 and per.year_id = p_yearid
) Meetingtime"
classPeriods	meetingTimes	endTime		Y	END_TIME	end_time	
classPeriods	officialAttendancePeriod				Daily_Attendance_Code	Daily_Attendance_Code	
locations	classroomIdentificationCode			Y	ROOM	SECTIONS.room	
locations	schoolId			Y	SCHOOLID	State-assigned value based on SECTIONS.schoolid	
locations	maximumNumberOfSeats					ROOM.occupancy_maximum (when set), or SECTIONS.maxenrollment (when set)	
![image](https://user-images.githubusercontent.com/105658542/170743768-b6d0add1-c826-4035-a84e-87b82a0a0403.png)
