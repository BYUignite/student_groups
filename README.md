# student_groups code

Arrange students into groups for lab or project work.  User sets the number of
students, the number of labs (or group rotations), and the nominal group size.
If students don't fit evenly into the groups, then a few groups will have one
extra student.

In the first lab/rotation, a simple sequential grouping is made since this is
arbitrary.  In subsequent rotations, students are randomized among groups so as to maximize the number of different partners (minimize redundant pairings). This is not perfect, so the code is run multiple times to find the minimum among the trials sampled (it quits early if the redundancy is zero.)

For even groupings, if we have 15 students in groups of 3, then we have "15 choose 3" = 455 possible groupings. If we have 4 lab rotations, then we would have 455 choose 4 = 1762351955 possibilities. We then select one (of many, likely) that has the minimum redundancy. This brute-force approach isn't practical, so the approximate method above is used. 

The algorithm works as follows.

Consider 10 students in 3 groups with three Labs/rotations
      
            Groups Lab 1     Groups, Lab 2
student_0:       1
student_1:       1
student_2:       1
student_3:       2
student_4:       2
student_5:       2
student_6:       3
student_7:       3
student_8:       3
student_9:       3

To set the groups for Lab 2, we have all students available. We randomly select a student for Group 1, say student 3.

            Groups Lab 1     Groups, Lab 2
student_0:       1
student_1:       1
student_2:       1
student_3:       2                1
student_4:       2
student_5:       2
student_6:       3
student_7:       3
student_8:       3
student_9:       3

Student 3 has worked with students 4 and 5, so we have students (0, 1, 2, 6, 7, 8, 9) available. Randomly select one of these for group 1, say student 8.

            Groups Lab 1     Groups, Lab 2
student_0:       1
student_1:       1
student_2:       1
student_3:       2                1
student_4:       2
student_5:       2
student_6:       3
student_7:       3
student_8:       3                1
student_9:       3

Students in group 1 are now student 3 and student 8, and they have worked with students 4, 5, 6, 7, and 9, so we have students (0,1,2) available; we randomly select student 2.

            Groups Lab 1     Groups, Lab 2
student_0:       1
student_1:       1
student_2:       1                1
student_3:       2                1
student_4:       2
student_5:       2
student_6:       3
student_7:       3
student_8:       3                1
student_9:       3

Now we set group 2. We have students (0, 1, 4, 5, 6, 7, 9) available, and we randomly choose one of these, say, student 0:
            Groups Lab 1     Groups, Lab 2
student_0:       1                2
student_1:       1
student_2:       1                1
student_3:       2                1
student_4:       2
student_5:       2
student_6:       3
student_7:       3
student_8:       3                1
student_9:       3

Student 0 has worked with students 1, and 2, so we have students (4, 5, 6, 7, 9) available. We randomly select student 6:
            Groups Lab 1     Groups, Lab 2
student_0:       1                2
student_1:       1
student_2:       1                1
student_3:       2                1
student_4:       2
student_5:       2
student_6:       3                2
student_7:       3
student_8:       3                1
student_9:       3

Students 0 and 6 have worked with students 1, 2, 7, 8, 9, so only students 4, and 5 are available (noting that others have either been partnered, or already selected for a group). Choose student 5:
            Groups Lab 1     Groups, Lab 2
student_0:       1                2
student_1:       1
student_2:       1                1
student_3:       2                1
student_4:       2
student_5:       2                2
student_6:       3                2
student_7:       3
student_8:       3                1
student_9:       3

Group 3 will now include whoever is left, but the algorithm proceeds as above. If there are no choices for avoiding previous pairings, then a random choice of students who have not yet been assigned to a group is made. 
            Groups Lab 1     Groups, Lab 2
student_0:       1                2
student_1:       1                3
student_2:       1                1
student_3:       2                1
student_4:       2                3
student_5:       2                2
student_6:       3                2
student_7:       3                3
student_8:       3                1
student_9:       3                3

Note that this gives some redundancy: student 7 has been with student 9 twice, and vice-versa.

For a Lab 3 column, everything above is repeated, but we consider Lab 1 and Lab 2 when considering previous pairings.

