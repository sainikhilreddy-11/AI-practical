"""
Practical 1: Set up Python environment for AI and implement a simple reflex
             or goal-based agent in a toy environment.

File Name: practical_01_agent.py
How to run:
    python practicals/practical_01_agent/non_ui.py
"""

# ==============================================================================
# Step 1: Define Environment (Smart Room with Temperature & Cleanliness)
# ==============================================================================
class SmartRoom:
    """Simulates a smart room toy environment."""
    def __init__(self, temperature=28.0, is_dirty=True):
        self.temperature = float(temperature)
        self.is_dirty = bool(is_dirty)

    def sense(self):
        """Returns the current percept (temperature and cleanliness)."""
        return {"temperature": self.temperature, "is_dirty": self.is_dirty}

    def execute_action(self, action):
        """Updates the environment state based on the chosen action."""
        if action == "TURN_ON_AC":
            self.temperature -= 2.0
            print("  [Environment] AC activated -> Room temperature decreased by 2 deg C.")
        elif action == "TURN_ON_HEATER":
            self.temperature += 2.0
            print("  [Environment] Heater activated -> Room temperature increased by 2 deg C.")
        elif action == "CLEAN_ROOM":
            self.is_dirty = False
            print("  [Environment] Vacuum cleaner executed -> Room is now clean.")
        elif action == "STANDBY":
            print("  [Environment] System on standby -> Room state maintained.")


# ==============================================================================
# Step 2: Implement Simple Reflex Agent (Condition -> Action)
# ==============================================================================
class SimpleReflexAgent:
    """
    Reflex Agent decides actions based purely on condition-action rules:
    Condition: Is room dirty? -> Action: CLEAN_ROOM
    Condition: Temp > 25 deg C    -> Action: TURN_ON_AC
    Condition: Temp < 18 deg C    -> Action: TURN_ON_HEATER
    """
    def decide(self, percept):
        temp = percept["temperature"]
        is_dirty = percept["is_dirty"]

        if is_dirty:
            return "CLEAN_ROOM", "Condition: Dust present -> Action: Clean"
        elif temp > 25.0:
            return "TURN_ON_AC", f"Condition: Temp ({temp} deg C) > 25 deg C -> Action: AC ON"
        elif temp < 18.0:
            return "TURN_ON_HEATER", f"Condition: Temp ({temp} deg C) < 18 deg C -> Action: Heater ON"
        else:
            return "STANDBY", "Condition: All sensors nominal -> Action: Standby"


# ==============================================================================
# Step 3: Implement Goal-Based Agent (Current State + Goal -> Action Selection)
# ==============================================================================
class GoalBasedAgent:
    """
    Goal-Based Agent evaluates how actions bring the state closer to a defined goal:
    Goal: Target Temperature = 22 deg C and Cleanliness = Clean
    """
    def __init__(self, target_temp=22.0):
        self.target_temp = target_temp

    def is_goal_satisfied(self, percept):
        return (abs(percept["temperature"] - self.target_temp) <= 1.0) and (not percept["is_dirty"])

    def decide(self, percept):
        temp = percept["temperature"]
        is_dirty = percept["is_dirty"]

        if is_dirty:
            return "CLEAN_ROOM", f"Goal requires Clean room. Current: Dirty."
        
        diff = temp - self.target_temp
        if diff > 1.0:
            return "TURN_ON_AC", f"Goal is {self.target_temp} deg C. Current is {temp} deg C (diff: +{diff:.1f} deg C). AC moves state toward goal."
        elif diff < -1.0:
            return "TURN_ON_HEATER", f"Goal is {self.target_temp} deg C. Current is {temp} deg C (diff: {diff:.1f} deg C). Heater moves state toward goal."
        else:
            return "STANDBY", f"Goal achieved! Current state matches target state ({self.target_temp} deg C, Clean)."


# ==============================================================================
# Step 4: Main Execution and Comparison
# ==============================================================================
def main():
    print("=" * 65)
    print("AI PRACTICAL 1: SIMPLE REFLEX AND GOAL-BASED AGENTS")
    print("=" * 65)

    # Sample Input
    initial_temperature = 29.0
    initial_cleanliness = True  # True means Dirty
    target_temperature = 22.0

    print(f"\n[Sample Input]")
    print(f"Initial Temperature : {initial_temperature} deg C")
    print(f"Room Cleanliness    : {'Dirty' if initial_cleanliness else 'Clean'}")
    print(f"Agent Goal Target   : {target_temperature} deg C and Clean Room\n")

    # --- Part A: Simple Reflex Agent Demo ---
    print("-" * 50)
    print("RUNNING: Simple Reflex Agent (Condition -> Action)")
    print("-" * 50)
    room1 = SmartRoom(temperature=initial_temperature, is_dirty=initial_cleanliness)
    reflex_agent = SimpleReflexAgent()

    for step in range(1, 5):
        percept = room1.sense()
        action, reason = reflex_agent.decide(percept)
        print(f"Step {step}: Temp={percept['temperature']} deg C, Dirty={percept['is_dirty']}")
        print(f"  Decision : {action} | Reason: {reason}")
        room1.execute_action(action)
        if action == "STANDBY":
            print("  Reflex agent reached comfort boundary.")
            break

    # --- Part B: Goal-Based Agent Demo ---
    print("\n" + "-" * 50)
    print("RUNNING: Goal-Based Agent (State + Target Goal -> Action)")
    print("-" * 50)
    room2 = SmartRoom(temperature=initial_temperature, is_dirty=initial_cleanliness)
    goal_agent = GoalBasedAgent(target_temp=target_temperature)

    for step in range(1, 6):
        percept = room2.sense()
        action, reason = goal_agent.decide(percept)
        print(f"Step {step}: Temp={percept['temperature']} deg C, Dirty={percept['is_dirty']}")
        print(f"  Decision : {action}")
        print(f"  Reason   : {reason}")
        room2.execute_action(action)
        if goal_agent.is_goal_satisfied(room2.sense()):
            print(f"  >>> SUCCESS: Goal state satisfied at Step {step}!")
            break

    print("\n" + "=" * 65)
    print("COMPARISON SUMMARY:")
    print("Reflex Agent     : Direct condition-action mapping, no goal foresight.")
    print("Goal-Based Agent : Evaluates actions against an explicit future goal state.")
    print("=" * 65)


if __name__ == "__main__":
    main()
