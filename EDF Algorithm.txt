import matplotlib.pyplot as plt
import numpy as np

def edf(tasks):
    tasks.sort(key=lambda x: (x[0], x[1], x[2]))  # Sort tasks by release_time, deadline, execution_time

    schedule = []
    current_time = 0

    for task in tasks:
        if current_time < task[0]:  # If the task hasn't missed its deadline
            start_time = max(current_time, task[0])
            end_time = start_time + task[2]
            schedule.append((task[3], start_time, end_time))
            current_time = end_time
        else:
            # If the task has missed its deadline, still include it in the schedule
            start_time = current_time
            end_time = start_time + task[2]
            schedule.append((task[3], start_time, end_time))
            current_time = end_time
            print(f"Task {task[3]} missed its deadline.")

    return schedule

def draw_gantt_chart(schedule):
    fig, ax = plt.subplots()

    for task in schedule:
        ax.barh(y=task[0], width=task[2] - task[1], left=task[1], height=0.6, edgecolor='black')

    ax.set_xlabel('Time')
    ax.set_ylabel('Tasks')
    ax.set_yticks(np.arange(len(schedule)))
    ax.set_yticklabels([task[0] for task in schedule])
    ax.invert_yaxis()  # Invert y-axis to have Task 1 at the top

    plt.show()

if __name__ == "__main__":
    # Format for each task: (release_time, deadline, execution_time, task_name)
    tasks = [
        (0, 5, 2, 'Task1'),
        (1, 4, 3, 'Task2'),
        (2, 6, 1, 'Task3'),
        (3, 7, 2, 'Task4'),
        (4, 9, 2, 'Task5'),
        (5, 8, 1, 'Task6'),
    ]

    schedule = edf(tasks)
    print("EDF Schedule:")
    for task in schedule:
        print(f"{task[0]}: Start Time={task[1]}, End Time={task[2]}")

    draw_gantt_chart(schedule)
