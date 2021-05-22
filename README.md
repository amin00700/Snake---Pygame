# snake with pygame
# SnAke game with a simple menu
import pygame as PG, sys, random
import pygame.locals as PG_LOCALS
import pygame.event as PG_EVENTS

PG.init()

width = 800 
hight = 600 
sc = PG.display.set_mode((width, hight))
PG.display.set_caption("ZHAAKHAR GAMES: SnAkE")

#colors
yellow = (125, 125, 0)
darkgray = (30, 30, 30)
green = (0, 185, 0)
red = (200, 0, 0)
cyan = (0, 255, 255)
lose = (139, 0, 0)
black = (0, 0, 0)
# game variables
x, y = 0, 0
speed = 20
fps = 10
score = 0
eated = True
click = False
click2 = False
click3 = False
start = False
dif1 = False
dif2 = False
dif3 = False
direction = "right"
change_to = direction
food_pos = [random.randrange(0, 380, 20), random.randrange(0, 260, 20)]
snake_pos = [180, 160]  
snake_Body = [snake_pos, [160, 160]]
font = PG.font.Font(None, 75)
font_score = PG.font.Font(None, 30)
text = font.render("test", True, cyan)
clock = PG.time.Clock()
# Soundtrack
# You can replace xxx with your own soundtrack
# soundtrack = PG.mixer.music.load("xxx.mp3")
# PG.mixer.music.play(-1)

def menu():
    global x, y, sc, click, lose, cyan, click2, click3
    while True:
        font = PG.font.Font(None, 25)
        text1 = font.render('Start', True, cyan)
        text2 = font.render("Dificulty", True, cyan)
        text3 = font.render("Exit", True, cyan)
        text4 = font.render("PRODUCER : AMINam", True, cyan)
        sc.fill(darkgray)
        for event in PG_EVENTS.get():

            if event.type == PG_LOCALS.QUIT:
                exit_game()

            if event.type == PG_LOCALS.MOUSEBUTTONDOWN:
                if event.button == 1 and 340 < x < 440 and 180 < y < 200:
                    click = True
                if event.button == 1 and 340 < x < 440 and 280 < y < 300:
                    click2 = True
                if event.button == 1 and 340 < x < 440 and 380 < y < 400:
                    click3 = True

        x, y = PG.mouse.get_pos()
        
        But1 = PG.Rect(340, 180, 100, 20)
        But2 = PG.Rect(340, 280, 100, 20)
        But3 = PG.Rect(340, 380, 100, 20)
        PG.draw.rect(sc, lose, But1)
        PG.draw.rect(sc, lose, But2)
        PG.draw.rect(sc, lose, But3)
        sc.blit(text1, (370, 182))
        sc.blit(text2, (355, 281))
        sc.blit(text3, (370, 382))
        sc.blit(text4, (15, 580))
        if But1.collidepoint((x, y)):
            if click:
                Main_game()

        if But2.collidepoint((x, y)):
            if click2:
                dificulty()
        
        if But3.collidepoint((x, y)):    
            if click3:
                sc.fill(darkgray)
                exit_game()
        PG.display.update()

def game_board():
    b = 20
    for i in range(39):
        PG.draw.line(sc, yellow, (b, 40), (b, hight))
        PG.draw.line(sc, yellow, (0, b+20), (width, b+20))
        b += 20

def dificulty():
    global fps, score, darkgray, lose, cyan, x, y, dif1, dif2, dif3
    myfont = PG.font.Font(None, 25)
    Kids = myfont.render("Kids", True, cyan)
    Men = myfont.render("Men", True, cyan)

    Legends = myfont.render("Legends", True, cyan)
    kids = PG.Rect(340, 180, 100, 20)
    men = PG.Rect(340, 280, 100, 20)
    legends = PG.Rect(340, 380, 100, 20)

    while True:
        sc.fill(darkgray)
        for event in PG_EVENTS.get():

            if event.type == PG_LOCALS.QUIT:
                exit_game()

            if event.type == PG_LOCALS.MOUSEBUTTONDOWN:
                if event.button == 1 and 340 < x < 440 and 180 < y < 200:
                    dif1 = True
                    dif2 = False
                    dif3 = False
                if event.button == 1 and 340 < x < 440 and 280 < y < 300:
                    dif1 = False
                    dif2 = True
                    dif3 = False
                if event.button == 1 and 340 < x < 440 and 380 < y < 400:
                    dif1 = False
                    dif2 = False
                    dif3 = True
        
        if dif1:
            fps = 13
            return fps
        if dif2:    
            fps = 18
            return fps
        if dif3:
            fps = 25
            return fps
        PG.draw.rect(sc, lose, kids)
        PG.draw.rect(sc, lose, men)
        PG.draw.rect(sc, lose, legends)
        sc.blit(Kids, (370, 182))
        sc.blit(Men, (370, 282))
        sc.blit(Legends, (360, 382))
        
        PG.display.update()

def move():
    global snake_Body, snake_pos, change_to, direction, speed, width, hight
    if change_to == 'up' and direction != 'down':
        direction = 'up'
    if change_to == 'down' and direction != 'up':
        direction = 'down'
    if change_to == 'left' and direction != 'right':
        direction = 'left'
    if change_to == 'right' and direction != 'left':
        direction = 'right'

    # Moving the snake
    if direction == 'up':
        snake_pos[1] -= speed
    if direction == 'down':
        snake_pos[1] += speed
    if direction == 'left':
        snake_pos[0] -= speed
    if direction == 'right':
        snake_pos[0] += speed

    
    if snake_pos[1] <= 20:
        snake_pos[1] = hight-20
    if snake_pos[1] >= hight:
        snake_pos[1] = 40
    if snake_pos[0] >= width:
        snake_pos[0] = 0
    if snake_pos[0] <= -20:
        snake_pos[0] = width

def exit_game():
    global sc, cyan, font
    text = font.render("You Exit The Game :((", True, cyan)
    sc.blit(text, (125, 240))
    PG.display.update()
    PG.time.delay(900)
    PG.quit()
    sys.exit()

def game_over():
    global sc, font, lose, score, darkgray
    text = font.render("!!!YOU LOOOOSE!!!", True, lose)
    text2 = font.render("Your score = " + str(score), True, lose)
    sc.fill(darkgray)
    sc.blit(text, (170, 230))
    sc.blit(text2, (230, 340))
    PG.display.update()
    PG.time.delay(1100)
    PG.quit()
    sys.exit()

    
    PG.display.update()

def Main_game():
    global eated, snake_Body, snake_pos, sc, darkgray, red, clock, cyan, food_pos, score, font, change_to, direction
    while True:
    
        sc.fill(darkgray)
        clock.tick(fps)

        for event in PG_EVENTS.get():
            if event.type == PG_LOCALS.QUIT:
                exit_game()
            if event.type == PG_LOCALS.KEYDOWN:
                if event.key == PG_LOCALS.K_d:
                    change_to = "right"
                if event.key == PG_LOCALS.K_a:
                        change_to = "left"
                if event.key == PG_LOCALS.K_w:
                    change_to = "up"
                if event.key == PG_LOCALS.K_s:
                    change_to = "down"    


        if not eated:
            food_pos = [random.randrange(0, width-20, 20), random.randrange(60, hight-40, 20)]
        eated = True

        snake_Body.insert(0, list(snake_pos))
        if snake_pos[0] == food_pos[0] and snake_pos[1] == food_pos[1]:
            score += 1
            eated = False
        else:
            snake_Body.pop()

        for pos in snake_Body:
            PG.draw.rect(sc, green, PG.Rect(pos[0], pos[1], 20, 20))

        # Snake Eyes
        PG.draw.rect(sc, black, PG.Rect(snake_pos[0]+2, snake_pos[1]+2, 3, 3))
        PG.draw.rect(sc, black, PG.Rect(snake_pos[0]+7, snake_pos[1]+7, 3, 3))

        PG.draw.rect(sc, red, PG.Rect(food_pos[0], food_pos[1], 20, 20))

        for block in snake_Body[2:]:
            if snake_pos[0] == block[0] and snake_pos[1] == block[1]:
                game_over()



        scc = font_score.render("Score : " + str(score), True, cyan)
        sc.blit(scc, (20 , 15))    
        move()
        # game_board()
        PG.display.update()

menu()
