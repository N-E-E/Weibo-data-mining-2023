[简体中文](./README-zh.md)

# Data Mining Competition(Topic 3)

This is the Graph Construction & Processing Code of 2023 Weibo Communication Data Mining Competition.

Due to confidentiality agreements, complete dataset cannot be provided. A small portion of the CSV file is available in the "./data" folder to show the data structure.

## Instructions
[July 15, 2023] Graph construction code is completed, and relevant interfaces for the graph data structure are provided.

### Directory Structure
```
./build --              Output graph files and some intermediate files
    csv/ :                  Neo4j visualization input
    graph_bin_data/ :       Graph data binary files
    json/ ：                Graph data in JSON format
./data --               Original provided data
./img --                Output images
./script --             Source code
    build_for_graph/ :      Neo4j scripts
    graph/ :                Graph construction source code
    analyse.ipynb :         Graph data analysis script
```

### Usage
```bash
cd scripts/graph
./run.bat
```
Execute the above script to generate the corresponding graph for the "CCTV Spring Festival Gala A Platform Data Dataset," stored in the "build/graph_bin_data" and "build/json" folders.

- Modify the command line parameters in the script according to your needs. The parameters are as follows:
```bash
--mode : generate-Generate the basic graph, profile-Analyze the graph for pruning, prune-Prune the graph based on degrees. Generally, only 'generate' is used.
--file_dir : Initial data folder
--graph_bin_path : Path for the graph binary file
--batch_size : Number of CSV files to generate one graph. Usually 1 is faster. Starting from 3, it takes a long time.
--json_path : Path for the graph JSON file
--prune_rate : Pruning rate. By default, keep nodes corresponding to degrees above max_deg*0.7
--profile_result_path : Path to save analysis results
--prune_result_path : Path to save pruning results
```

- JSON Format Explanation:

    Basic structure:
    ```json
    {
        "nodes" : {
            "user1" : {
                "forward_edges" : [],
                "forward_by_edges": []
            },
            "user2" : {
                "forward_edges" : [],
                "forward_by_edges": []
            }
            ...
        }
    }
    ```
    Where `useri` is a user node, `forward_edges` is a list, recording outgoing edges (i.e., who user1 forwarded to), and `forward_by_edges` is a list, recording incoming edges (i.e., who forwarded to user1). The specific structure is shown below:

    ```json
    "Kellyyeahaaa": {
          "forward_edges": [
            {
              "from_user": "Kellyyeahaaa",
              "to_user": "吉儿奶奶",
              "content": "轮她[小雪人]",
              "comment_num": 0.0,
              "likes_num": 0.0,
              "forward_num": 0,
              "weight": 0.0
            },
            {
              "from_user": "Kellyyeahaaa",
              "to_user": "T0ourn3oll·",
              "content": "来啦！",
              "comment_num": 0.0,
              "likes_num": 0.0,
              "forward_num": 0,
              "weight": 0.0
            }
          ],
          "forward_by_edges": [
            {
              "from_user": "T0ourn3oll·",
              "to_user": "Kellyyeahaaa",
              "content": "",
              "comment_num": 0.0,
              "likes_num": 0.0,
              "forward_num": 0,
              "weight": 0.0
            },
            {
              "from_user": "走一走一zoey",
              "to_user": "Kellyyeahaaa",
              "content": "期待龚俊",
              "comment_num": 0.0,
              "likes_num": 0.0,
              "forward_num": 0,
              "weight": 0.0
            }
          ]
        }
    ```
    The above data indicates that `Kellyyeahaaa` forwarded content from users `吉儿奶奶` and `T0ourn3oll·`, and content posted by `Kellyyeahaaa` was forwarded by `走一走一zoey` and `T0ourn3oll·`. The content's information, including content/forward_num/like_num/comment_num, is recorded. The `weight` is calculated based on the sum of forward_num, like_num, and comment_num.

- If file names appear as garbled characters during execution, you can directly modify the 17th line of `build_total_graph.py`:
```python
parser.add_argument("--file_dir", default="../../data/央视春晚A平台数据", help="which root file to build graph for")
```
- Generally, only the CSV table of the A platform can construct a graph. You can check the JSON file for the corresponding graph content.

