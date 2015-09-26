from pulp import *
  
# list of teachers
teachers = ["Aya",
          "Tamami",
          "Nanae",
          "Patrick",
          "Noriko"]
  
#  Weight for the dedication for teaching
weights = {#Teacher weights
          "Aya":1.2,
          "Tamami":1.2,
          "Nanae":1.2,
          "Patrick":1,
          "Noriko":1
	  }
days = ["day 1",
        "day 2",
        "day 3",
        "day 4",
        "day 5",
        "day 6"]
  
times = ["9-10",
        "10-11",
        "11-12",
        "13-14",
        "14-15",
        "15-16"]

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
lesson = [(d, t, i, st) for d in days for t in times for i in teachers for st in students]
  
# The student-teacher maching into the dictionary
matching = makeDict([teachers, students],stmatching,0)
  
# Creates the problem variables of the class schedules
classes = LpVariable.dicts("Classes",(days,times, teachers, students),0,1,LpInteger)
  
# Creates the 'prob' variable to contain the problem data
prob = LpProblem("Music Summer class schedule Problem",LpMaximize)
  
# The objective function is added to prob - The sum of value of music summer school
prob += lpSum(classes[d][t][i][st]*matching[i][st]*weights[i] for (d, t, i,st) in lesson), "value of music summer school"
  
# Add constraint 1: One teacher can only teach one student at each time.
for d in days:
	for t in times:
		for i in teachers:
			prob += lpSum(classes[d][t][i][st] for st in students)<=1, "One teacher can teach one student at a time.%s%s%s%s" %(d, t, i, st)

# Add constraint 2: One teacher can teach no more than 5 lessons per day
for d in days:
	for i in teachers:
		prob += lpSum(classes[d][t][i][st] for st in students for t in times)<=5, "One teacher can teach no more than 5 lessons per day.%s%s%s%s" %(d, t, i, st)

# The problem data is written to an .lp file
prob.writeLP("MusicSummerScheduleProgram.lp")
  
# The problem is solved using PuLP's choice of Solver
prob.solve()
  
# The status of the solution is printed to the screen
print("Status:", LpStatus[prob.status])
  
# Each of the variables is printed with it's resolved optimum value
for v in prob.variables():
    print(v.name, "=", v.varValue)
  
# The optimised objective function value is printed to the screen    
print("Value of Music Summer School = ", value(prob.objective))
