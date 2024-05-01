# DDD


```python

import pygame
pygame.init()


back = (200, 255, 255) #цвет фона (background)
mw = pygame.display.set_mode((500, 500)) #окно программы (main window)
mw.fill(back)
clock = pygame.time.Clock()


#переменные, отвечающие за координаты платформы
racket_x = 200
racket_y = 330


#флаг окончания игры
game_over = False
#класс из предыдущего проекта
class Area():
   def __init__(self, x=0, y=0, width=10, height=10, color=None):
       self.rect = pygame.Rect(x, y, width, height)
       self.fill_color = back
       if color:
           self.fill_color = color


   def color(self, new_color):
       self.fill_color = new_color


   def fill(self):
       pygame.draw.rect(mw, self.fill_color, self.rect)


   def collidepoint(self, x, y):
       return self.rect.collidepoint(x, y)       


   def colliderect(self, rect):
       return self.rect.colliderect(rect)








```
