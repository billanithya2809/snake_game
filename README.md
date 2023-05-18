# snake_game
import pygame
import random

# Initialize the Pygame module
pygame.init()

# Set up the game window
WIDTH = 640
HEIGHT = 480
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Define colors
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Define the size of each grid cell
GRID_SIZE = 20
GRID_WIDTH = WIDTH // GRID_SIZE
GRID_HEIGHT = HEIGHT // GRID_SIZE

# Define the initial position and direction of the snake
snake_x = GRID_WIDTH // 2
snake_y = GRID_HEIGHT // 2
snake_direction = "right"

# Initialize the snake's body as a list of segments
snake_body = [(snake_x, snake_y)]

# Generate the initial position of the food
food_x = random.randint(0, GRID_WIDTH - 1)
food_y = random.randint(0, GRID_HEIGHT - 1)

# Initialize the score
score = 0

# Set up the game clock
clock = pygame.time.Clock()

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        # Change the snake's direction based on keypresses
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and snake_direction != "down":
                snake_direction = "up"
            elif event.key == pygame.K_DOWN and snake_direction != "up":
                snake_direction = "down"
            elif event.key == pygame.K_LEFT and snake_direction != "right":
                snake_direction = "left"
            elif event.key == pygame.K_RIGHT and snake_direction != "left":
                snake_direction = "right"

    # Update the snake's position
    if snake_direction == "up":
        snake_y -= 1
    elif snake_direction == "down":
        snake_y += 1
    elif snake_direction == "left":
        snake_x -= 1
    elif snake_direction == "right":
        snake_x += 1

    # Check for collision with the food
    if snake_x == food_x and snake_y == food_y:
        # Increase the score
        score += 1

        # Generate a new position for the food
        food_x = random.randint(0, GRID_WIDTH - 1)
        food_y = random.randint(0, GRID_HEIGHT - 1)

        # Extend the snake's body
        snake_body.append((snake_x, snake_y))

    # Check for collision with the snake's body or the game edges
    if (snake_x, snake_y) in snake_body[:-1] or \
            snake_x < 0 or snake_x >= GRID_WIDTH or \
            snake_y < 0 or snake_y >= GRID_HEIGHT:
        running = False

    # Add the new head to the snake's body
    snake_body.append((snake_x, snake_y))

    # Remove the tail segment if the snake hasn't eaten food
    if len(snake_body) > score + 1:
        del snake_body[0]

    # Clear the game window
    window.fill(BLACK)

    # Draw the snake
    for segment in snake_body:
        pygame.draw.rect(window, GREEN, (segment[0] * GRID_SIZE, segment[1] * GRID_SIZE, GRID_SIZE, GRID_SIZE))

    # Draw the food
    pygame.draw.rect(window, RED, (food_x * GRID_SIZE, food_y * GRID_SIZE, GRID_SIZE, GRID_SIZE))

    # Update the display
    pygame.display.flip()

    # Set the game speed
    clock.tick(4)

# Quit the game
pygame.quit()
