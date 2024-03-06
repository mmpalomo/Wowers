mport React, { useState } from 'react';
import { View, TextInput, Button, FlatList, Text, StyleSheet, ScrollView, TouchableOpacity } from 'react-native';

const GoalApp = () => {
  const [goal, setGoal] = useState('');
  const [goalsList, setGoalsList] = useState([]);
  const [isHidden, setIsHidden] = useState(false);
  const [editingIndex, setEditingIndex] = useState(null);

  const addGoalHandler = () => {
    if (editingIndex !== null) {
      // Editing existing goal
      const updatedGoals = [...goalsList];
      updatedGoals[editingIndex].value = goal;
      setGoalsList(updatedGoals);
      setEditingIndex(null);
    } else {
      // Adding new goal
      const randomColor = getRandomColor();
      setGoalsList([...goalsList, { key: Math.random().toString(), value: goal, color: randomColor }]);
    }

    setGoal('');
  };

  const deleteGoalsHandler = () => {
    setGoalsList([]);
  };

  const toggleVisibilityHandler = () => {
    setIsHidden(!isHidden);
  };

  const startEditing = (index) => {
    setGoal(goalsList[index].value);
    setEditingIndex(index);
  };

  const renderItem = ({ item, index }) => {
    return (
      <View style={{ ...styles.goalItem, backgroundColor: item.color }}>
        <Text style={styles.bullet}>•</Text>
        <Text style={styles.goalText}>{item.value}</Text>
        <View style={styles.buttonsContainer}>
          <TouchableOpacity onPress={() => iDeleteMo(index)}>
            <Text style={styles.deleteButton}>X</Text>
          </TouchableOpacity>
          <TouchableOpacity onPress={() => itagoMoAko(index)}>
            <Text style={styles.hideButton}>{isHidden ? 'SHOW' : 'HIDE'}</Text>
          </TouchableOpacity>
          <TouchableOpacity onPress={() => startEditing(index)}>
            <Text style={styles.editButton}>EDIT</Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  };

  const getRandomColor = () => {
    const letters = '0123456789ABCDEF';
    let color = '#';
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  };

  const iDeleteMo = (index) => {
    const updatedGoals = [...goalsList];
    updatedGoals.splice(index, 1);
    setGoalsList(updatedGoals);
  };

  const itagoMoAko = (index) => {
    const updatedGoals = [...goalsList];
    updatedGoals[index].isVisible = !updatedGoals[index].isVisible;
    setGoalsList(updatedGoals);
  };

  return (
    <View style={styles.container}>
      <View style={styles.inputContainer}>
        <TextInput
          placeholder="Enter your wish or goal"
          style={styles.input}
          onChangeText={(text) => setGoal(text)}
          value={goal}
        />
        <Button title={editingIndex !== null ? 'Edit Goal' : 'Add Goal'} onPress={addGoalHandler} />
      </View>
      <ScrollView>
        {isHidden ? null : (
          <FlatList
            data={goalsList}
            renderItem={renderItem}
            keyExtractor={(item) => item.key}
          />
        )}
      </ScrollView>
      <View style={styles.buttonContainer}>
        <TouchableOpacity onPress={toggleVisibilityHandler}>
          <Text style={styles.hideButton}>{isHidden ? 'SHOW' : 'HIDE'}</Text>
        </TouchableOpacity>
        <Button title="Delete All" onPress={deleteGoalsHandler} />
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 30,
  },
  inputContainer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  input: {
    width: '70%',
    borderBottomColor: 'black',
    borderBottomWidth: 1,
    padding: 10,
  },
  goalItem: {
    padding: 10,
    marginVertical: 5,
    borderColor: 'black',
    borderWidth: 1,
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-between',
  },
  bullet: {
    fontSize: 20,
    marginRight: 10,
  },
  goalText: {
    flex: 1,
    fontSize: 16,
  },
  buttonsContainer: {
    flexDirection: 'row',
  },
  deleteButton: {
    color: 'black',
    fontSize: 20,
  },
  hideButton: {
    color: 'black',
    fontSize: 16,
    marginLeft: 10,
  },
  editButton: {
    color: 'blue',
    fontSize: 20,
    marginLeft: 10,
  },
  buttonContainer: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginTop: 10,
  },
});

export default GoalApp;
