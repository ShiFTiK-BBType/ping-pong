import pygame
import time
from random import randint

pygame.init()

back = (200, 255, 255) 
window = pygame.display.set_mode((550, 600)) 
window.fill(back) 
clock = pygame.time.Clock() 

racket_x = 50
racket_y = 250
racket_x2 = 450
racket_y2 = 250
speed = 35
dx = 1
dy = 1

move_right = False
move_left = False


class Area():
    def __init__(self, x=0, y=0, width=10, height=10, color=None):
        self.rect = pygame.Rect(x, y, width, height)
        self.fill_color = back
        if color:
            self.fill_color = color

    def color(self, new_color):
        self.fill_color = new_color

    def fill(self):
        pygame.draw.rect(window, self.fill_color, self.rect)

    def collidepoint(self, x, y):
        return self.rect.collidepoint(x, y)      

    def colliderect(self, rect):
        return self.rect.colliderect(rect)


class Picture(Area):
    def __init__(self, filename, x=0, y=0, width=10, height=10):
        super().__init__(x=x, y=y, width=width, height=height, color=None)
        self.image = pygame.image.load(filename)
      
    def draw(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Label(Area):

    def set_text(self, text, fsize=12, color=(0, 0, 0)):
        self.font = pygame.font.SysFont('verdana', fsize) 
        self.image = self.font.render(text,True, color) 

    def draw(self, shift_x=0, shift_y=0):
        self.fill() 
        window.blit(self.image,(self.rect.x + shift_x, self.rect.y + shift_y)) 



keys = pygame.key.get_pressed()
ball = Picture('ball.png', 200, 100, 50, 50)
platform = Picture('platform.png', racket_x, racket_y, 30, 100)
platform1 = Picture('platform.png', racket_x2, racket_y2, 30, 100)
game_over = False
game = True


lose1 = Label(250, 250, 50, 50, back)
lose1.set_text('PLAYER 1 LOSE!', True, (100, 0, 0))
lose2 = Label(250, 250, 50, 50, back)
lose2.set_text('PLAYER 2 LOSE!', True, (100, 0, 0))
#lose2 = font.render('PLAYER 2 LOSE!', True, (100, 0, 0))

while game:
    ball.fill()
    platform.fill()
    platform1.fill()
    ball.rect.x += dx
    ball.rect.y += dy
    if ball.rect.x > (racket_x + 45):
        time_text = Label(150, 150, 50, 50, back)
        time_text.set_text('Пинг-Понг', 60, (200, 0, 0))
        time_text.draw(10, 10)
        game_over = True

    if ball.rect.x > (racket_x2 + 45):
        time_text = Label(150, 250, 50, 50, back)
        time_text.set_text('Player 2 LOSE!', 50, (255 , 0, 0))
        time_text.draw(10,10)
        game_over = True
    if ball.rect.x < 0:
        time_text = Label(150, 250, 50, 50, back)
        time_text.set_text('Player 1 LOSE!', 50, (255 , 0, 0))
        time_text.draw(10,10)
        game_over = True
    
    if ball.rect.y > 550 or ball.rect.y < 0:
        dy *= -1
    if ball.colliderect(platform) or ball.colliderect(platform1):
        dx *= -1
        dy *= 1

    if ball.colliderect(platform.rect):
        dy *= -1
    if ball.rect.y < 0:
        dy *= -1
    if ball.rect.x > 450 or ball.rect.x < 0:
        dy *= -1

    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game = False
            
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_w:
                platform.rect.y -= speed
            if event.key == pygame.K_s:
                platform.rect.y += speed
        
            if event.key == pygame.K_UP:
                platform1.rect.y -= speed
            if event.key == pygame.K_DOWN:
                platform1.rect.y += speed


    platform1.draw()
    platform.draw()
    ball.draw()
    pygame.display.update()
    clock.tick(144)



    



    
    
    
    
    
    
    
    
    
    
    
    


