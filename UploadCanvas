from canvasapi import Canvas
api_key = "yourkeyhere" # Put your API key here
base_url = "https://unomaha.instructure.com" # Change this to your Canvas URL
canvas = Canvas(base_url, api_key)


import pandas as pd
df = pd.read_excel('grades.xlsx') # Make sure to change 'grades.xlsx' to your actual file name
my_courses = df['Course Number'].unique().tolist() 


for course_id in my_courses: # Loop through each course, if uploading a single course, set course_id to your course ID and remove the loop
    course = canvas.get_course(course_id)
    assignment_map = {}
    df_course = df[df['Course Number'] == course_id]  # Filter DataFrame for the current course
    for assignment in course.get_assignments(): # Find assignment names in course
        assignment_map[assignment.name] = assignment.id
    for _, row in df.iterrows():
        user_id = row['SIS Login ID']
        for assignment_name in df.columns[6:]:  # Skip to the columns with assignment names in excel sheet
            if assignment_name in assignment_map: # Make sure the assignment in excel matches a course assignment
                assignment_id = assignment_map[assignment_name]
                grade = row[assignment_name]
                if pd.notna(grade):  # Only upload if the grade is not NaN
                    assignment = course.get_assignment(assignment_id)
                    submission = assignment.get_submission(user_id)
                    submission.edit(submission={'posted_grade': grade})

