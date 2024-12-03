import pygame
import sys

# Pygame initialisieren
pygame.init()

# Fenstergrößen und Farben
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Spielfenster erstellen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Mario Level")

# Spielfigur (Mario) Eigenschaften
player_width = 50
player_height = 50
player_x = 100
player_y = SCREEN_HEIGHT - player_height - 50
player_velocity_x = 0
player_velocity_y = 0
player_speed = 5
gravity = 1

# Plattformen
platforms = [
    pygame.Rect(0, SCREEN_HEIGHT - 50, SCREEN_WIDTH, 50),  # Boden
    pygame.Rect(200, SCREEN_HEIGHT - 150, 200, 20),        # Plattform 1
    pygame.Rect(500, SCREEN_HEIGHT - 250, 200, 20),        # Plattform 2
]

# Sprungstatus
is_jumping = False

# Spiel Schleife
clock = pygame.time.Clock()
while True:
    # Ereignisbehandlung
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Tasten eingabe
    keys = pygame.key.get_pressed()

    # Bewegung des Spielers
    if keys[pygame.K_LEFT]:
        player_velocity_x = -player_speed
    elif keys[pygame.K_RIGHT]:
        player_velocity_x = player_speed
    else:
        player_velocity_x = 0

    # Sprungmechanik
    if keys[pygame.K_SPACE] and not is_jumping:
        player_velocity_y = -15
        is_jumping = True

    # Schwerkraft anwenden
    player_velocity_y += gravity

    # Spieler Position aktualisieren
    player_x += player_velocity_x
    player_y += player_velocity_y

    # Kollision mit Plattformen
    player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
    for platform in platforms:
        if player_rect.colliderect(platform) and player_velocity_y > 0:
            if player_rect.bottom > platform.top:
                player_y = platform.top - player_height
                player_velocity_y = 0
                is_jumping = False

    # Fenster füllen
    screen.fill(WHITE)

    # Plattformen zeichnen
    for platform in platforms:
        pygame.draw.rect(screen, BLUE, platform)

    # Spieler (Mario) zeichnen
    pygame.draw.rect(screen, RED, player_rect)

    # Bildschirm aktualisieren
    pygame.display.flip()

    # FPS (Frames per Second) setzen
    clock.tick(60)
