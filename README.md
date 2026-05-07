import pygame
import random

pygame.init()

screen = pygame.display.set_mode((400, 500))
font = pygame.font.SysFont("Arial", 24)

red_mars = (150, 50, 0)
white = (255, 255, 255)
black = (0, 0, 0)

player_x = 50
player_y = 200
player_speed = 0

rock_x = 400
rock_y = 300

score = 0
high_score = 0
attempts = 1

game_running = True
while game_running:
    screen.fill(black)
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                player_speed = -7

    player_speed = player_speed + 0.4
    player_y = player_y + player_speed

    rock_x = rock_x - 5
    if rock_x < -50:
        rock_x = 400
        rock_y = random.randint(150, 400)
        score = score + 1
        if score > high_score:
            high_score = score

    if player_x + 30 > rock_x and player_x < rock_x + 50:
        if player_y + 25 > rock_y or player_y < rock_y - 150:
            player_y = 200
            player_speed = 0
            rock_x = 400
            score = 0
            attempts = attempts + 1

    if player_y > 500 or player_y < -50:
        player_y = 200
        player_speed = 0
        rock_x = 400
        score = 0
        attempts = attempts + 1

    pygame.draw.rect(screen, white, (player_x, player_y, 30, 30))
    pygame.draw.rect(screen, red_mars, (rock_x, rock_y, 50, 500))
    pygame.draw.rect(screen, red_mars, (rock_x, rock_y - 650, 50, 500))
    
    s_txt = font.render("Score: " + str(score), True, white)
    h_txt = font.render("Best: " + str(high_score), True, white)
    a_txt = font.render("Attempts: " + str(attempts), True, white)
    
    screen.blit(s_txt, (10, 10))
    screen.blit(h_txt, (10, 40))
    screen.blit(a_txt, (10, 70))
    
    pygame.display.update()
    pygame.time.Clock().tick(60)

pygame.quit()
