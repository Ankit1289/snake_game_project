# snake_game_project
import pygame 
import random
import os
pygame.init()
pygame.mixer.init()
pygame.init()

white=(255,255,255)
red=(255,0,0)
black=(0,0,0)

#display width height 
screen_width=900
screen_hight=600
gamewindow=pygame.display.set_mode((screen_width,screen_hight))
#
bgimg = pygame.image.load("image/snake1.jpg")
bgimg = pygame.transform.scale(bgimg, (screen_width, screen_hight)).convert_alpha()
bgimg1 = pygame.image.load("image/snake.jpg")
bgimg1 = pygame.transform.scale(bgimg, (screen_width, screen_hight)).convert_alpha()

pygame.display.set_caption("Snake Game")
pygame.display.update()



clock=pygame.time.Clock()
font= pygame.font.SysFont(None,55)



def text_screen(text,color,x,y):
    screen_text=font.render(text,True,color)
    gamewindow.blit(screen_text,[x,y])

def plot_snake(gamewindow,color,snk_list,snake_size):
    for x,y in snk_list:
        pygame.draw.rect(gamewindow,color,[x,y,snake_size,snake_size])

def welcome():
    exit_game= False
    while not exit_game:
        gamewindow.fill((255,255,255))
        gamewindow.blit(bgimg, (0, 0))
        text_screen("Welcome To Snakes",black,250,250)
        text_screen("Press Space bar To Play",black,220,300)
        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                exit_game=True
                
            if event.type==pygame.KEYDOWN:
                if event.key==pygame.K_SPACE:
                    pygame.mixer.music.load('sound/back.mp3')
                    pygame.mixer.music.play()
                    
                    gameloop()
        pygame.display.update()
        clock.tick(30)

def gameloop():
    exit_game=False
    game_over=False
    if(not os.path.exists("highscore.txt")):
        with open("highscore.txt", "w") as f:
            f.write("0")


    with open("highscore.txt","r") as f:
        highscore=f.read()

    snake_x=60
    snake_y=70
    velocity_x=0
    velocity_y=0
    init_velocity=5

    snk_list=[]
    snk_length=1

    score=0
    food_x=random.randint(20,screen_width)
    food_y=random.randint(20,screen_hight)
    snake_size=20
    fps=30
    while not exit_game:
        if game_over:
            
            with open("highscore.txt", "w") as f:
                f.write(str(highscore))
            
           
            gamewindow.blit(bgimg, (0, 0))
            text_screen("game over!  Press enter to continue",red,150,250)
            

            for event in pygame.event.get():
                
                if event.type==pygame.QUIT:
                    exit_game=True
                if event.type==pygame.KEYDOWN:
                    if event.key==pygame.K_RETURN:
                        welcome()

        else:
            for event in pygame.event.get():
                #giving the speed to snake
                if event.type==pygame.QUIT:
                    exit_game=True
                if event.type==pygame.KEYDOWN:
                    if event.key==pygame.K_RIGHT:
                        velocity_x=init_velocity

                        velocity_y=0
                    if event.key==pygame.K_LEFT:
                        velocity_x=-init_velocity

                        velocity_y=0
                    
                    if event.key==pygame.K_UP:
                        velocity_y=-init_velocity
                        velocity_x=0
                    
                    if event.key==pygame.K_DOWN:
                        velocity_y=init_velocity
                        velocity_x=0

            snake_x=snake_x+velocity_x
            snake_y=snake_y+velocity_y
            if abs(snake_x-food_x)<10 and abs(snake_y-food_y)<10:
                score+=10
                food_x=random.randint(20,screen_width)
                food_y=random.randint(20,screen_hight)
                snk_length +=5
                
                if score>int(highscore):
                    highscore=score
                    
            
            gamewindow.blit(bgimg1, (0, 0))
            
            text_screen("score: "+str(score)+"                highscore: "+ str(highscore),red,5,5)
            pygame.draw.rect(gamewindow,red,[food_x,food_y,snake_size,snake_size])
            
            head=[]
            head.append(snake_x)
            head.append(snake_y)
            snk_list.append(head)

            if len(snk_list)> snk_length:
                del snk_list[0]
            if head in snk_list[:-1]:
                game_over=True
                
                
            
            if snake_x<0 or snake_x>screen_width or snake_y<0 or snake_y>screen_hight:
                game_over=True
                
               

            
            #pygame.draw.rect(gamewindow,black,[snake_x,snake_y,snake_size,snake_size])

            plot_snake(gamewindow,black,snk_list,snake_size,)
        pygame.display.update()
        clock.tick(fps)

    pygame.quit()
    quit()
welcome()
