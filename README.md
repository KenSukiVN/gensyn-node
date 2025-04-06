# How to run multiple swarm nodes if you have multiple GPUs

Example you have 2 GPUs

1. Stop your node ( attach to screen and ctrl + C)
2. start your node again by use first GPU : 

`CUDA_VISIBLE_DEVICES=0 ./run_rl_swarm.sh`

Open another terminal :
1. Clone repo again to different folder : 

`git clone https://github.com/gensyn-ai/rl-swarm rl-swarm-2 && cd rl-swarm-2`

2. Copy to new script file to run : `cp run_rl_swarm.sh run_rl_swarm_2.sh`
3. Run command below "

```
export MODAL_PORT=3001
export SWARM_PORT=38332

sed -i "s|MODAL_PROXY_URL = \"http://localhost:3000/api/\"|MODAL_PROXY_URL = \"http://localhost:${MODAL_PORT}/api/\"|g" hivemind_exp/chain_utils.py

sed -i "s|/tcp/38331|/tcp/${SWARM_PORT}|g" run_rl_swarm_2.sh
sed -i "s|yarn dev > /dev/null 2>&1 &|yarn dev -p ${MODAL_PORT} > /dev/null 2>&1 &|g" run_rl_swarm_2.sh
sed -i "s|open http://localhost:3000|open http://localhost:${MODAL_PORT}|g" run_rl_swarm_2.sh
```
4. Start your node : 

```
python3 -m venv .venv
source .venv/bin/activate
CUDA_VISIBLE_DEVICES=1 ./run_rl_swarm_2.sh
```
5. Login again use port-forward or tunnel to port 3001
