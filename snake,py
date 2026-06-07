import random
from ursina import *

GRID_SIZE = 20
CELL_SIZE = 1
window.size = (600, 600)
app = Ursina()
camera.orthographic = True
camera.fov = 20
camera.position = (0, 0, -30)

class Snake():
    def __init__(self):
        self.move_timer = 0 # tracks how much time has passed since the last move
        self.move_interval = 0.15  # move every 0.15 seconds
        self.snake = [(1, 0), (0, 0), (-1, 0)]
        self.direction = (1, 0)
        self.entities = []
        self.game_over = False
        Entity(model='quad', color=color.red, scale=(20, 0.1), y=9.5)   # top
        Entity(model='quad', color=color.red, scale=(20, 0.1), y=-9.5)  # bottom
        Entity(model='quad', color=color.red, scale=(0.1, 20), x=9.5)   # right
        Entity(model='quad', color=color.red, scale=(0.1, 20), x=-9.5)  # left

    def draw(self):
        for e in self.entities:
            destroy(e)
        self.entities = []

        for x,y in self.snake:
            part = Entity(model='quad', color=color.green, scale=CELL_SIZE,)
            part.x = x
            part.y = y
            self.entities.append(part)

    def move(self):
        if self.game_over:
            return False
        
        hx, hy = self.snake[0]
        dx, dy = self.direction
        new_head = (hx + dx, hy + dy)
        nx, ny = new_head
        if nx < -9 or nx > 9 or ny < -9 or ny > 9:
            Text(text='YOU DIED!', position=(0, 0.2), origin=(0, 0), scale=2)
            self.game_over = True
            print(f"died at {new_head}")
            return False
        elif new_head in self.snake[1:]:
            Text(text='YOU DIED!', position=(0, 0.2), origin=(0, 0), scale=2)
            self.game_over = True
            print(f"died at {new_head}")
            return False
        self.snake.insert(0, new_head)

        if new_head == (food.x, food.y):
            food.draw(self.snake)
        else:
            del self.snake[-1]


class Food:
    def __init__(self):
        self.food = Entity(model='quad', color=color.red, scale=1)
        self.x = 0
        self.y = 0
        self.draw([])

    def draw(self, snake_body):
        while True:
            rx = random.randint(-9, 9)
            ry = random.randint(-9, 9)
            if (rx, ry) not in snake_body:
                self.x = rx
                self.y = ry
                self.food.x = rx
                self.food.y = ry
                break
        

snake = Snake()
food = Food()


def update():
    if snake.game_over:
        return
    snake.move_timer += time.dt # add the time since the last frame to the timer. 
    if snake.move_timer >= snake.move_interval:
        snake.move_timer = 0
        snake.move()
    snake.draw()


    if held_keys['w'] and snake.direction != (0, -1):
        snake.direction = (0, 1)
    elif held_keys['s'] and snake.direction != (0, 1):
        snake.direction = (0, -1)
    elif held_keys['d'] and snake.direction != (-1, 0):
        snake.direction = (1, 0)
    elif held_keys['a'] and snake.direction != (1, 0):
        snake.direction = (-1, 0)


app.run()
