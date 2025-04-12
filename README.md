import pygame
import random
import time

# Initialize pygame
pygame.init()

# Set up game window
WIDTH, HEIGHT = 600, 400
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Cyber Security Awareness Game")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Font setup
font = pygame.font.SysFont("Arial", 24)
large_font = pygame.font.SysFont("Arial", 36)

# Questions and Answers
questions = [
    {
        "question": "What is phishing?",
        "choices": ["A type of fishing", "A social engineering attack", "An online shopping site", "A type of virus"],
        "correct": 1
    },
    {
        "question": "Which of the following is a strong password?",
        "choices": ["123456", "password", "P@ssw0rd!2025", "admin123"],
        "correct": 2
    },
    {
        "question": "What does VPN stand for?",
        "choices": ["Virtual Private Network", "Virtual Protected Node", "Very Protected Network", "Virtual Personal Network"],
        "correct": 0
    },
    {
        "question": "What is two-factor authentication?",
        "choices": ["A system for tracking login attempts", "A process where two passwords are used", "An extra layer of security for online accounts", "A type of encryption"],
        "correct": 2
    },
    {
        "question": "What should you do if you receive a suspicious email from an unknown sender?",
        "choices": ["Open the attachments to check", "Click on any links to investigate", "Delete it immediately", "Reply asking for more information"],
        "correct": 2
    }
]

# Game Variables
score = 0
question_index = 0
time_limit = 15  # Time in seconds for each question
game_running = True

# Function to display text
def display_text(text, font, color, x, y):
    label = font.render(text, True, color)
    window.blit(label, (x, y))

# Function to handle game over screen
def game_over():
    global score
    window.fill(WHITE)
    display_text("Game Over!", large_font, RED, WIDTH // 4, HEIGHT // 4)
    display_text(f"Your Score: {score}", font, BLACK, WIDTH // 3, HEIGHT // 2)
    pygame.display.update()
    time.sleep(3)

# Function to handle question screen
def question_screen():
    global question_index, score
    if question_index < len(questions):
        question = questions[question_index]
        window.fill(WHITE)
        display_text(question["question"], font, BLACK, 20, 50)
        
        # Shuffle choices
        choices = random.sample(question["choices"], len(question["choices"]))
        for i, choice in enumerate(choices):
            display_text(f"{i + 1}. {choice}", font, BLACK, 20, 100 + i * 40)
        
        pygame.display.update()

        # User input
        start_time = time.time()
        answered = False
        while not answered:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    exit()
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_1:
                        answer = 0
                    elif event.key == pygame.K_2:
                        answer = 1
                    elif event.key == pygame.K_3:
                        answer = 2
                    elif event.key == pygame.K_4:
                        answer = 3
                    else:
                        continue
                    
                    if answer == question["correct"]:
                        score += 1
                    answered = True
                    break
            
            # Check time limit
            if time.time() - start_time > time_limit:
                answered = True
                display_text("Time's up!", font, RED, WIDTH // 3, HEIGHT // 2)
                pygame.display.update()
                time.sleep(2)
                break

        question_index += 1
    else:
        game_over()

# Main Game Loop
def main_game():
    global game_running
    while game_running:
        window.fill(WHITE)
        display_text("Cyber Security Awareness Game", large_font, GREEN, 50, 100)
        display_text("Press any key to start", font, BLACK, 150, 200)
        pygame.display.update()
        
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                exit()
            if event.type == pygame.KEYDOWN:
                question_screen()

# Start the game
main_game()
