import random
import csv
import os
from datetime import datetime

FILE_NAME = "interview_history.csv"

questions = [
    {
        "question": "Tell me about yourself.",
        "keywords": ["student", "experience", "skills", "education"]
    },
    {
        "question": "What are your strengths?",
        "keywords": ["teamwork", "leadership", "communication", "problem"]
    },
    {
        "question": "Explain Object-Oriented Programming.",
        "keywords": ["class", "object", "inheritance", "polymorphism"]
    },
    {
        "question": "What is Python?",
        "keywords": ["language", "programming", "interpreted", "object"]
    },
    {
        "question": "Difference between List and Tuple?",
        "keywords": ["mutable", "immutable"]
    }
]

def create_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Question", "Score"])

def evaluate(answer, keywords):
    score = 0
    answer = answer.lower()

    for word in keywords:
        if word in answer:
            score += 20

    return min(score, 100)

def start_interview():
    score_total = 0

    print("\n===== AI Mock Interview =====")

    for i in range(5):
        q = random.choice(questions)

        print(f"\nQuestion {i+1}")
        print(q["question"])

        answer = input("Your Answer: ")

        score = evaluate(answer, q["keywords"])

        score_total += score

        with open(FILE_NAME, "a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([
                datetime.now().strftime("%Y-%m-%d %H:%M"),
                q["question"],
                score
            ])

        print("Score:", score, "/100")

    average = score_total / 5

    print("\n========== RESULT ==========")
    print("Average Score:", average)

    if average >= 80:
        print("Excellent Performance!")
    elif average >= 60:
        print("Good Performance!")
    elif average >= 40:
        print("Needs Improvement!")
    else:
        print("Practice More!")

def view_history():
    print("\n===== Interview History =====")

    with open(FILE_NAME, "r") as file:
        reader = csv.reader(file)

        for row in reader:
            print(row)

def menu():
    create_file()

    while True:
        print("\n========== AI INTERVIEW PREPARATION ==========")
        print("1. Start Mock Interview")
        print("2. View Interview History")
        print("3. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            start_interview()

        elif choice == "2":
            view_history()

        elif choice == "3":
            print("Thank you!")
            break

        else:
            print("Invalid Choice!")

if __name__ == "__main__":
    menu()
