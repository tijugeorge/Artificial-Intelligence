# Artificial Intelligence Nanodegree
## Introductory Project: Diagonal Sudoku Solver

# Question 1 (Naked Twins)
Q: How do we use constraint propagation to solve the naked twins problem?  
A: Constraint propagation is the technique to apply multiple or same constraints repeatedly on the problem, to solve it. 
This technique is applied until a solution is found or we no longer able to apply the constraint. 
In naked twin’s problem, we apply the naked twin as a strategy to reduce the number of possibilities. 
Consider the following:

 ![naked_twin](https://github.com/tijugeorge/Artificial-Intelligence/blob/master/Sudoku_with_AI/naked_twin.png)
 
We have identified 23 in F3 and I3 and we are sure one of them belongs to either of the box and both belong to the same set of peers. 
The strategy is to eliminate the numbers, here 2 and 3 from all the boxes that have F3 and I3 as peers. 

Implementation in python:
First is to identify all boxes with twin elements and then to find for those that can form naked twins. 
Once naked twins are identified we remove the corresponding digits form all the boxes that are peers to the twin elements.
```python
def naked_twins(values):
    """Eliminate values using the naked twins strategy.
    Args:
        values(dict): a dictionary of the form {'box_name': '123456789', ...}
    Returns:
        the values dictionary with the naked twins eliminated from peers.
    """
    # Find all instances of naked twins
    # Eliminate the naked twins as possibilities for their peers
    #find the boxes with 2 entries
    twins = [box for box in values.keys() if len(values[box]) == 2]
    #Boxes containing similar elements
    naked_twins = [[box1, box2] for box1 in twins \
                    for box2 in peers[box1] \
                    if set(values[box1]) == set(values[box2])]
    #This is for every pair of naked twins
    for i in range(len(naked_twins)):
        box1 = naked_twins[i][0]
        box2 = naked_twins[i][1]
        #Find intersection of peers
        peers1 = set(peers[box1])
        peers2 = set(peers[box2])
        peers_intersection = peers1 & peers2
        #For all common peers delete the two digits
        for peer in peers_intersection:
            if len(values[peer])>2:
                for rem_val in values[box1]:
                    values = assign_value(values, peer, values[peer].replace(rem_val, ''))
    return values
```

# Question 2 (Diagonal Sudoku)
Q: How do we use constraint propagation to solve the diagonal sudoku problem?  
A: For diagonal sudoku problem we can implement this constraint as a deciding factor in the beginning of the problem. 
Consider the code snippet

```python
diagonal1_unit = [[rows[i]+cols[i] for i in range(len(rows))]]
diagonal2_unit = [[rows[i]+cols_reverse[i] for i in range(len(rows))]]

check = 1 # check is kept 1 for diagonal sudoku and 0 for non-diagonal
"To check if the it is a diagonal soduko"
if check == 1:
    unitlist = row_units + column_units + square_units + diagonal1_unit + diagonal2_unit
else:
    unitlist = row_units + column_units + square_units
```
We start with diagonal as an additional unit and it will be part of the peers. If it doesn’t satisfy the diagonal constraint then solutions won’t be accepted. 

### Install

This project requires **Python 3**.

We recommend students install [Anaconda](https://www.continuum.io/downloads), a pre-packaged Python distribution that contains all of the necessary libraries and software for this project. 
Please try using the environment we provided in the Anaconda lesson of the Nanodegree.

##### Optional: Pygame

Optionally, you can also install pygame if you want to see your visualization. If you've followed our instructions for setting up our conda environment, you should be all set.

If not, please see how to download pygame [here](http://www.pygame.org/download.shtml).

### Code

* `solution.py` - You'll fill this in as part of your solution.
* `solution_test.py` - Do not modify this. You can test your solution by running `python solution_test.py`.
* `PySudoku.py` - Do not modify this. This is code for visualizing your solution.
* `visualize.py` - Do not modify this. This is code for visualizing your solution.

### Visualizing

To visualize your solution, please only assign values to the values_dict using the `assign_value` function provided in solution.py

### Submission
Before submitting your solution to a reviewer, you are required to submit your project to Udacity's Project Assistant, which will provide some initial feedback.  

The setup is simple.  If you have not installed the client tool already, then you may do so with the command `pip install udacity-pa`.  

To submit your code to the project assistant, run `udacity submit` from within the top-level directory of this project.  You will be prompted for a username and password.  If you login using google or facebook, visit [this link](https://project-assistant.udacity.com/auth_tokens/jwt_login) for alternate login instructions.

This process will create a zipfile in your top-level directory named sudoku-<id>.zip.  This is the file that you should submit to the Udacity reviews system.

