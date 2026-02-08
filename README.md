# Advanced AI-Based Task Scheduler
# Developed as an original architecture-level developer example

from datetime import datetime, timedelta
import heapq
import uuid


class Task:
    def __init__(self, title, deadline, importance, estimated_hours):
        self.id = str(uuid.uuid4())
        self.title = title
        self.deadline = deadline
        self.importance = importance
        self.estimated_hours = estimated_hours
        self.created_at = datetime.now()

    def priority_score(self):
        """AI-like scoring logic based on urgency + importance + time"""
        time_left = (self.deadline - datetime.now()).total_seconds() / 3600
        urgency_factor = max(1, 100 / (time_left + 1))
        return self.importance * urgency_factor

    def __lt__(self, other):
        return self.priority_score() > other.priority_score()


class SmartScheduler:
    def __init__(self):
        self.tasks = []

    def add_task(self, title, deadline, importance, estimated_hours):
        task = Task(title, deadline, importance, estimated_hours)
        heapq.heappush(self.tasks, task)
        print(f"Task Added: {task.title}")

    def generate_schedule(self):
        print("\n--- Optimized Work Schedule ---")
        current_time = datetime.now()
        temp_tasks = self.tasks.copy()
        heapq.heapify(temp_tasks)

        while temp_tasks:
            task = heapq.heappop(temp_tasks)
            end_time = current_time + timedelta(hours=task.estimated_hours)

            print(f"""
Task: {task.title}
Start: {current_time.strftime('%Y-%m-%d %H:%M')}
End:   {end_time.strftime('%Y-%m-%d %H:%M')}
Priority Score: {round(task.priority_score(),2)}
            """)

            current_time = end_time


# Example Usage
if __name__ == "__main__":
    scheduler = SmartScheduler()

    scheduler.add_task(
        "Build AI module",
        datetime.now() + timedelta(hours=20),
        importance=9,
        estimated_hours=5
    )

    scheduler.add_task(
        "Database optimization",
        datetime.now() + timedelta(hours=40),
        importance=7,
        estimated_hours=4
    )

    scheduler.add_task(
        "Client presentation",
        datetime.now() + timedelta(hours=10),
        importance=10,
        estimated_hours=3
    )

    scheduler.generate_schedule()
