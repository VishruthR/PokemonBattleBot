# Pokemon Battle Bot 

This repository allows for users to compare pokemon battle bots performance. Currently it supports the [foul-play](https://github.com/VishruthR/foul-play#) with MCTS using UCB, Regret Matching, and Exp3. 

### How to use

First clone all submodules 
```
git clone --recurse-submodules git@github.com:VishruthR/PokemonBattleBot.git
```

#### Pokemon Showdown Set-up

Follow the instructions in [pokemon-showdown/server/README](https://github.com/smogon/pokemon-showdown/blob/master/server/README.md) to start the Showdown server. It should be running on `localhost:8000`.

#### Foul play set Set-up
```
cd foul-play
pip install -r requirements.txt # I recommend doing this in a Python venv
make poke_engine_local GEN=gen6 # You will need Rust for this
mkdir logs 
```

#### Running foul-play

Open the Showdown server on `localhost:8000`. In the top right select `Choose name` and create a name for your bot to play under (e.g. `bot_1_498_test`), do not enter a password. If the name is taken you will need to come up with another. Log out and create another player just like you did for `bot_1_498_test` (e.g. create `bot_2_498_test`). This step registers two accounts which can now challenge each other. 

The following command will connect `foul-play`'s bot to the Pokemon Showdown server on `localhost:8000`, log it in with the name `bot_1_498_test`, challenge the user with the name `bot_2_498_test` to a gen6 random battle using the UCB exploration method. 
```
python run.py \
    --websocket-uri ws://localhost:8000/showdown/websocket \ # assuming ur running on localhost:8000
    --ps-username 'bot_1_498_test' \
    --bot-mode challenge_user \
    --user-to-challenge 'bot_2_498_test' \
    --pokemon-format gen6randombattle \
    --search-time-ms 500 \ # how long to run MCTS for
    --run-count 1 \ # how many battles to run
    --search-type ucb # ucb, rm, exp3
```
You can access more options for `run.py` with the `--help` flag. 

If you want to play the bot, you can log in with the `--user-to-challenge` name and then run the command above. IMPORTANTLY, the `--user-to-challenge` must be logged in BEFORE you run the above command.

If you want two bots to play each other, open a new terminal and run:
```
python run.py \
    --websocket-uri ws://localhost:8000/showdown/websocket \
    --ps-username 'bot_2_498_test' \
    --bot-mode accept_challenge \
    --pokemon-format gen6randombattle \
    --search-time-ms 100 \
    --run-count 1 \
    --search-type ucb
```

Ensure you run this command to log `bot_2_498_test` in, BEFORE you run the command for `bot_1_498_test`.
