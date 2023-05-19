# Ping-pong
from pygame import *

mixer.init()
mixer.music.load('katona.ogg') 
mixer.music.play()
fire_music = mixer.Sound('fire.ogg')

class Sprite_game(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, speed, wight, height):
        super().__init__()
        self.image = transform.scale(image.load(player_image),(55, 55))
        self.speed = speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(Sprite_game):
    def update_right(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < 420:
            self.rect.y += self.speed
    def update_left(self):
        keys = key.get_pressed()
        if keys[K_RIGHT] and self.rect.y > 5:
            self.rect.x -= self.speed
        if keys[K_LEFT] and self.rect.y < 420:
            self.rect.x += self.speed

window = display.set_mode((700, 500))
display.set_caption("Ping pong")
background = transform.scale(image.load("tabl.jpg"), (700, 500))# фон изображение если обыч фон не нужен Transform sceil
hero = Player("rocket.png", 5, 400, 80, 100, 10)

timer = time.Clock()
FPS = 60 #!
game = True
conec = False

racket1 = Player('racketka.png', 30, 200, 60, 50, 150)# изображение ракетки
racket2 = Player('racketka.png', 500, 200, 60, 50, 150)
ball = Sprite_game('ball.png', 200, 200, 60 ,50, 50)

font.init()
font1 = font.Font(None, 36)
pobeda = font1.render('Игрок 1 проиграл!', True, (190, 0, 0))
proigrysh = font1.render('Игрок 2 проиграл!', True, (190, 0, 0))
proigrysh1 = font1.render('Вы проиграли!', True, (190, 0, 0))
propusk = 0
popal = 0
max_propusk = 3
popadanie = 10

puli = sprite.Group()

speed_y = 2
speed_x = 2

while game:
    for i in event.get():
        if i.type == QUIT:
            game = False
        if i.type == KEYDOWN:
            if i.key == K_SPACE:
                fire_music.play()
                hero.fire()
    
    if not conec:
        window.blit(background, (0, 0))
        racket1.update_left()
        racket2.update_right()
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            speed_x *= -1
            speed_y *= 1

        if ball.rect.y > 450 or ball.rect.y < 0:
            speed_y *= -1

        if ball.rect.x < 0:
            conec = True
            window.blit(proigrysh1, (200, 200))
            game = False

        if ball.rect.x > 700:
            conec = True
            window.blit(proigrysh1, (200, 200))
            game = False

        hero.update()
        racket1.reset()
        racket2.reset()
        ball.reset()

    display.update()
    timer.tick(FPS)
