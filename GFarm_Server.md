DFX-Gfram-server
- user : dfx_3
- pw : RsTSkjw1d62wh3d
- host : 211.107.220.176
- post : 24
- ssh dfx_3@211.107.220.176 -p 24
- docker run -d --name pig_data -v /home/dfx_3/ruripian:/workspace/pig_data_ruripian pig_data
 - scp -P 24 dfx_3@211.107.220.176:home/dfx_3/pig_data3.zip 
 - scp pig_data.tar dfx_3@211.107.220.176:home/dfx_3
-  tmux new -s ruripian