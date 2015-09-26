from pulp import *
# list of rooms
rooms = ["Rm1",
          "Rm2",
          "Rm3",
          "Rm4",
          "Rm5",
          "Rm6",
          "Rm7",
          "Rm8"]
  
# Room-teacher maching
rtmatching = [  #Romms
         #r1 r2 r3 r4 r5 r6 r7 r8
         [1, 1, 1, 1, 1, 1, 1, 1], #Aya
         [1, 0, 0, 1, 0, 1, 0, 0], #Tamami Teacher
         [1, 1, 1, 1, 1, 1, 1, 1], #Nanae
         [1, 1, 1, 1, 1, 1, 1, 1], #Patrick
         [1, 1, 0, 1, 0, 1, 0, 0]  #Noriko
         ]

# list of teachers
teachers = ["Aya",
          "Tamami",
          "Nanae",
          "Patrick",
          "Noriko"]
  
#  Weight for the dedication for teaching
weights = {#Teacher weights
          "Aya":0.7,
          "Tamami":1.2,
          "Nanae":1.2,
          "Patrick":1,
          "Noriko":1
	  }
days = ["D1",
        "D2",
        "D3",
        "D4",
        "D5",
        "D6"]
  
times = ["9a",
        "10a",
        "11a",
        "1p",
        "2p",
        "3p",
        "4p"]


students = ["s1",
          "s2",
          "s3",
          "s4",
          "s5",
          "s6",
          "s7",
          "s8",
          "s9",
          "s10"]
  
# students-teacher maching
stmatching = [  #Students
         #s1 s2 s3 s4 s5 s6 s7 s8 s9 s10
         [1, 1, 0, 0, 0, 0, 0, 0, 1, 0], #Aya
         [0, 0, 1, 1, 1, 0, 0, 0, 0, 0], #Tamami Teacher
         [0, 0, 1, 0, 0, 1, 1, 1, 1, 0], #Nanae
         [1, 1, 0, 0, 0, 0, 0, 0, 1, 0], #Patrick
         [0, 0, 0, 1, 0, 0, 0, 0, 0, 1]  #Noriko
         ]

# Creates all the possible combination for the class schedule
lesson = [(d, t, i, st, r) for d in days for t in times for i in teachers for st in students for r in rooms]
  
# The room-teacher maching into the dictionary
rtmatch = makeDict([teachers, rooms],rtmatching,0)
  
# The student-teacher maching into the dictionary
stmatch = makeDict([teachers, students],stmatching,0)

# Creates the problem variables of the class schedules
classes = LpVariable.dicts("Classes",(days,times, teachers, students, rooms),0,1,LpInteger)
  
# Creates the 'prob' variable to contain the problem data
prob = LpProblem("Music Summer class schedule Problem",LpMaximize)
  
# The objective function is added to prob - The sum of value of music summer school
prob += lpSum(classes[d][t][i][st][r]*stmatch[i][st]*weights[i]*rtmatch[i][r] for (d, t, i,st, r) in lesson), "value of music summer school"
  
# Add constraint 1: Each teacher can only teach one student at one room for each class.
for d in days:
	for t in times:
		for i in teachers:
			prob += lpSum(classes[d][t][i][st][r] for st in students for r in rooms)<=1, "Each teacher can teach one student at a time.%s%s%s%s%s" %(d, t, i, st, r)

# Add constraint 2: Each teacher can teach no more than 6 lessons per day
for d in days:
	for i in teachers:
		prob += lpSum(classes[d][t][i][st][r] for st in students for t in times for r in rooms)<=6, "Each teacher can teach no more than 6 lessons per day.%s%s%s%s%s" %(d, t, i, st, r)

# Add constraint 3: Each student can take up to 5 lessons per day
for d in days:
	for st in students:
		prob += lpSum(classes[d][t][i][st][r] for t in times for i in teachers for r in rooms)<=5, "Each student can take up to 5 lessons per day.%s%s%s%s%s" %(d, t, i, st, r)

# The problem data is written to an .lp file
prob.writeLP("MusicSummerScheduleProgram.lp")
  
# The problem is solved using PuLP's choice of Solver
prob.solve()
  
# The status of the solution is printed to the screen
print("Status:", LpStatus[prob.status])
  
# Each of the variables is printed with it's resolved optimum value
for v in prob.variables():
	if v.varValue >= 0.1:
    		print(v.name, "=", v.varValue)
# The optimised objective function value is printed to the screen    
print("Value of Music Summer School = ", value(prob.objective))
