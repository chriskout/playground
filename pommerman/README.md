# Our Pommerman Agent
**Authors:** Connor Onweller and Christopher Outhwaite

We are using a BDI approach to this problem.

## Beliefs:

We get out agents beliefs from the state of the world passed to our agent. Most important beliefs in through the obs array.
The following defined in `pommerman/agent/my_agent.py`:

```python
my_position = tuple(obs['position'])
board = np.array(obs['board'])
bombs = convert_bombs(np.array(obs['bomb_blast_strength']))
enemies = [constants.Item(e) for e in obs['enemies']]
ammo = int(obs['ammo'])
blast_strength = int(obs['blast_strength'])
items, dist, prev = self._djikstra(
    board, my_position, bombs, enemies, depth=10)
```

## Desires:

Our desires are represented by the if conditions located after the defintions of our beliefs.
They are created in a hieracle structure. Earlier defined desires are acted upon first. 
Our desires are as follows (listed Hierarchically in terms highest priority to lowest):

1. Move away from a bomb (if applicable)
2. Lay pomme if adjacent to an enemy
3. Move towards an enemy if there is exactly one in three reachable spaces
4. If only 1 other agent is alive move towards an enemy in 10 spaces
5. Move towards a good item if there is one within 4 spaces
6. If more then 2 agents are alive, move towards wooden wall if within 6 spaces
7. Maybe lay a bomb if next to a wooden wall
8. If within 10 blocks of an enemy, and we are at an intersection, place a bomb
9. Move towards a wooden wall if there is one within two reachable spaces and you have a bomb
10. Choose a random but valid direction that we have not recently visited

## Intentions:

Our agent commit to an intention moving down the desires hierarchically, executing the plan associated with the first desire consistent with the agents beliefe at that time. The plan the agent chooses represents the intention commits to at the current time step.
