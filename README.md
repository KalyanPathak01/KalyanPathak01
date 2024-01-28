Creating a simple Flappy Bird game with a GUI in Python requires a graphical library like Pygame. Here's a basic example using Pygame:


import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
GRAVITY = 0.5
JUMP_HEIGHT = 10

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Bird class
class Bird(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 4, HEIGHT // 2)
        self.velocity = 0

    def jump(self):
        self.velocity = -JUMP_HEIGHT

    def update(self):
        self.velocity += GRAVITY
        self.rect.y += self.velocity

# Pipe class
class Pipe(pygame.sprite.Sprite):
    def __init__(self, x):
        super().__init__()
        self.image = pygame.Surface((100, random.randint(100, 400)))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = 0

    def update(self):
        self.rect.x -= 5
        if self.rect.right < 0:
            self.kill()

# Initialize screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Create sprites
all_sprites = pygame.sprite.Group()
pipes = pygame.sprite.Group()
bird = Bird()
all_sprites.add(bird)

# Game loop
clock = pygame.time.Clock()
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird.jump()

    # Spawn pipes
    if random.randint(0, 100) < 5:
        pipe = Pipe(WIDTH)
        pipes.add(pipe)
        all_sprites.add(pipe)

    # Update sprites
    all_sprites.update()
    pipes.update()

    # Check for collisions
    hits = pygame.sprite.spritecollide(bird, pipes, False)
    if hits:
        running = False

    # Draw background
    screen.fill(BLACK)

    # Draw sprites
    all_sprites.draw(screen)
    pipes.draw(screen)

    # Update display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)

Make sure you have Pygame installed (`pip install pygame`) before running this code. You can modify and expand upon this code to enhance the game further.
