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


calendars	calendarCode			Y		Composite based on SCHOOLS.school_number and may have hyphen plus track letter appended if calendar describes a track.	
calendars	schoolReference	schoolId		Y	SCHOOL_NUMBER	SCHOOLS.school_number	
calendars	schoolYearTypeReference	schoolYear		Y		Based on context of data being published	
calendars	calendarTypeDescriptor			Y		Default to 'School'	Ed-Fi Descriptors
calendarDates	date			SC	DATE_VALUE	CALENDAR_DAY.date_value	
calendarDates	calendarReference	calendarCode		Y		Composite based on SCHOOLS.school_number and may have hyphen plus track letter appended if calendar describes a track.	
calendarDates	calendarReference	schoolId		Y	SCHOOL_NUMBER	State-assigned value based on SCHOOLS.school_number	 
calendarDates	calendarReference	schoolYear		Y		Based on context of data being published	
calendarDates	calendarEvents	calendarEventDescriptor		Y		CALENDAR_DAY.type, CALENDAR_DAY.insession , CALENDAR_DAY.MEMBERSHIPVALUE	"SC Ed-Fi Descriptors
The idea is that schools will have a maximum of three descriptors per day, In-Session, Membership and Type. If values exist in either or all of these fields, a descriptor should be written to the ODS for each field where a value exists."
![image](https://user-images.githubusercontent.com/105658542/170744247-76002fdc-fc47-4e2f-b23a-c39d49b14bd5.png)
