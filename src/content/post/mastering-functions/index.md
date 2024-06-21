---
title: "Mastering Functions"
publishDate: "15 April 2023"
description: "How to Write Functions That Will Take Your Skills to the Next Level"
coverImage:
  src: "./cover.png"
  alt: "Mastering Functions cover image"
tags: ["functions", "python"]
---



I'm on the verge of finishing a great course called "[Clean Code](https://www.udemy.com/course/writing-clean-code/)" by Maximilian Schwarzmüller.

I stumbled upon the concept of writing good and clean functions and wanted to share some valuable principles which will be especially handy for beginners. So you can see this blog post as an informative summary of that course section.

Let's dive in.

## Eight main points

1. Functions are like little helpers in our code that we can repeatedly use to do something specific.

2. A good function should only do one thing. It shouldn't be trying to do too much all at once.

3. We can also think about levels of abstraction, which means how high or low we are in terms of how much detail we deal with.

4. High-level operations are like big-picture ideas that we can express with a few words or phrases, while low-level operations are more detailed and specific.

5. A good function should only be responsible for work that is one level of abstraction below its name. This means that if the function has a descriptive name, it should only deal with operations that are one level more detailed than that name suggests. Use verbs or short phrases with adjectives to name functions correctly (e.g., `send_data`)

6. When we write a function, we should give it a descriptive name that tells us what it does.

7. If a function is doing too much, we can break it down into smaller functions that each does one specific thing.

8. By breaking down functions into smaller pieces, we can make our code easier to understand and maintain.

Consider this simple function that calls `collect_coin` and adds 1 to the player's coin count.:

```python
def collect_coin(player: dict) -> None:
    player['coins'] += 1

```

## Lousy function

Here's an example of a lousy function that's trying to do too much:

```python
def play_game(player: dict, enemies: list, walls: list) -> None:
    for enemy in enemies:
        if player['rect'].colliderect(enemy['rect']):
            player['hitpoints'] -= 1
            if player['hitpoints'] == 0:
                player['game_over']()
    for wall in walls:
        if player['rect'].colliderect(wall['rect']):
            player['move_back']()
    player['coins'] += 1

```

The function calls `play_game`, which checks for collisions with enemies and walls, adjusts the player's hitpoints and coin count, and even ends the game if the player's hitpoints reach zero.

## Smaller functions

To illustrate, we can fix the `play_game` function by breaking it down into smaller functions that each does one specific thing.

```python
def check_for_enemy_collision(player: dict, enemies: list) -> None:
    for enemy in enemies:
        if player['rect'].colliderect(enemy['rect']):
            player['hitpoints'] -= 1
            if player['hitpoints'] == 0:
                player['game_over']()

def check_for_wall_collision(player: dict, walls: list) -> None:
    for wall in walls:
        if player['rect'].colliderect(wall['rect']):
            player['move_back']()

def collect_coin(player: dict) -> None:
    player['coins'] += 1

def play_game(player: dict, enemies: list, walls: list) -> None:
    check_for_enemy_collision(player, enemies)
    check_for_wall_collision(player, walls)
    collect_coin(player)
```

### Side note

`rect` and `colliderect` are attributes and methods of a `Rect` object, which is a built-in class in the `pygame` module used for handling rectangular areas.

So, the expression `player.rect.colliderect(enemy.rect)`  checks if the player is hitting the enemy by comparing the position and size of the two objects using the `colliderect` method.
