import pygame
from sys import exit

def display_score():

    count_time = int(pygame.time.get_ticks()/1000) - start_time
    if game_active:
        score_surf = font.render("Score: " + f"{count_time}", False, (64, 64, 64))
        score_rect = score_surf.get_rect(center=(768, 150))
        screen.blit(score_surf, score_rect)
        return count_time

pygame.init()
screen = pygame.display.set_mode((1536, 814))
pygame.display.set_caption("Math game")
clock = pygame.time.Clock()
start_time = 0
game_active = False
score = 0
sky_surface = pygame.image.load('Picture Pygame/mario-background.jpg').convert()

font = pygame.font.Font("Picture Pygame/Pixeltype.ttf", 50)
#text_surface = font.render("  Math game ", False, (0, 95, 185))
#text_rect = text_surface.get_rect(midbottom=(768, 150))

game_name = font.render("MATH GAME", False, (111, 196, 169))
game_name_rect = game_name.get_rect(center=(768, 200))

press_space_to_start = font.render("Press space to start", False, (111, 196, 169))
press_space_to_start_rect = press_space_to_start.get_rect(center=(768, 600))

snail = pygame.image.load("Picture Pygame/snail1.png").convert_alpha()
snail_rect = snail.get_rect(midbottom=(1200, 716))

player_surface = pygame.image.load("Picture Pygame/player_walk_1.png").convert_alpha()
player_rect = player_surface.get_rect(midbottom=(200, 716))
player_gravity = 0  # Initial gravity velocity

player_stand = pygame.image.load("Picture Pygame/player_stand.png").convert_alpha()
player_stand = pygame.transform.rotozoom(player_stand,0,2)
player_stand_rect = player_stand.get_rect(center=(668, 400 ))


math_icon = pygame.image.load("Picture Pygame/math_3965108.png").convert_alpha()
math_icon_size = player_stand.get_size()
math_icon = pygame.transform.scale(math_icon, math_icon_size)
math_icon_rect = math_icon.get_rect(center=(868, 400))
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()

        if game_active:
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    # Jump when SPACE key is pressed
                    if player_rect.bottom == 716:  # Check if the player is on the ground
                        player_gravity = -20  # Set a negative velocity to move upwards
        else:
            if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
                game_active = True
                snail_rect.left = 1536
                start_time = int(pygame.time.get_ticks()/1000)

    if game_active:
        screen.blit(sky_surface, (0, 0))
#        pygame.draw.rect(screen, (255, 165, 0), text_rect, 16, 10)
#        screen.blit(text_surface, text_rect)

        score = display_score()
        screen.blit(snail, snail_rect)
        snail_rect.x -= 8
        if snail_rect.left < 0:
           snail_rect.left = 1536


        # Update player's position based on gravity
        player_gravity += 1  # Apply gravity
        player_rect.y += player_gravity

        # Keep the player on the ground
        if player_rect.bottom > 716:
            player_rect.bottom = 716
            player_gravity = 0  # Reset gravity when on the ground

        screen.blit(player_surface, player_rect)

    #    mouse_position = pygame.mouse.get_pos()
    #    if player_rect.collidepoint(mouse_position):
    #        print("Collision")

        if snail_rect.colliderect(player_rect):
            game_active = False
    else:
        screen.fill((94, 129, 162))
        screen.blit(player_stand, player_stand_rect)
        screen.blit(math_icon, math_icon_rect)
        score = display_score()
        score_message = font.render(f"Your score: {score} ", False,(94, 129, 162))
        score_message_rect = score_message.get_rect(center = (768, 600))
        screen.blit(game_name, game_name_rect)
        screen.blit(score_message,score_message_rect)
        if score == 0:
            screen.blit(press_space_to_start,press_space_to_start_rect)
        else:
            screen.blit(score_message,score_message_rect)
    pygame.display.update()
    clock.tick(60)
