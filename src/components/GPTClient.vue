<template>
<v-container fluid class="main-container">
    <div class="title-container">
        <img src="@/assets/nutrinus_logo.png" width="100px">
        <h2 class="title">NutriNUS Meal Plan<br>Generator Prototype</h2>
    </div>
    
    <!-- Button to trigger meal plan generation -->
    <div class="button-container">
        <v-btn @click="generateMealPlan" class="generate-button">
        Generate
        </v-btn>

        <!-- Show loading status -->
        <v-progress-circular v-if="isLoading" indeterminate color="primary"></v-progress-circular>
    </div>

    <h5><strong>This AI-powered prototype uses OpenAI's GPT 4o-mini LLM model.</strong>
        <br><strong>Instructions:</strong> Click <i>Generate</i> to generate new meal plan.
        <br>Hover over mealcards to view dish details.
        <br>Click specific mealcard to input and update existing meal.
        <br>Details for new meal will be updated in tooltip accordingly.
    </h5>
    <!-- Display the meal plan as a structured table -->
    <v-row v-if="mealPlan && mealPlan.length > 0" class="meal-plan-row">
    <v-simple-table>
        <tr class="meal-header">
            <th>Date</th>
            <th>Breakfast</th>
            <th>Lunch</th>
            <th>Dinner</th>
            <th>Snacks</th>
        </tr>
        <tr v-for="(day, index) in mealPlan" :key="day.date">
            <td class="date-cell">{{ day.date }}</td>
            
            <!-- Breakfast Column -->
            <td>
            <v-tooltip bottom class="food-tooltip">
                <template v-slot:activator="{ props }">
                <v-card class="meal-card" @click="editMeal(index, 'breakfast')" v-bind="props">
                    <v-card-text>{{ day.breakfast[0].dish_name }}</v-card-text>
                </v-card>
                </template>
                <span>
                Ingredients: {{ day.breakfast[0].ingredients }}<br>
                Calories: {{ day.breakfast[0].calories }} kcal<br>
                Protein: {{ day.breakfast[0].protein }}g<br>
                Carbs: {{ day.breakfast[0].carbs }}g<br>
                Fats: {{ day.breakfast[0].fats }}g
                </span>
            </v-tooltip>
            </td>

            <!-- Lunch Column -->
            <td>
            <v-tooltip bottom class="food-tooltip">
                <template v-slot:activator="{ props }">
                <v-card class="meal-card" @click="editMeal(index, 'lunch')" v-bind="props">
                    <v-card-text>{{ day.lunch[0].dish_name }}</v-card-text>
                </v-card>
                </template>
                <span>
                Ingredients: {{ day.lunch[0].ingredients }}<br>
                Calories: {{ day.lunch[0].calories }} kcal<br>
                Protein: {{ day.lunch[0].protein }}g<br>
                Carbs: {{ day.lunch[0].carbs }}g<br>
                Fats: {{ day.lunch[0].fats }}g
                </span>
            </v-tooltip>
            </td>

            <!-- Dinner Column -->
            <td>
            <v-tooltip bottom class="food-tooltip">
                <template v-slot:activator="{ props }">
                <v-card class="meal-card" @click="editMeal(index, 'dinner')" v-bind="props">
                    <v-card-text>{{ day.dinner[0].dish_name }}</v-card-text>
                </v-card>
                </template>
                <span>
                Ingredients: {{ day.dinner[0].ingredients }}<br>
                Calories: {{ day.dinner[0].calories }} kcal<br>
                Protein: {{ day.dinner[0].protein }}g<br>
                Carbs: {{ day.dinner[0].carbs }}g<br>
                Fats: {{ day.dinner[0].fats }}g
                </span>
            </v-tooltip>
            </td>

            <!-- Snacks Column -->
            <td>
            <v-tooltip bottom class="food-tooltip">
                <template v-slot:activator="{ props }">
                <v-card class="meal-card" @click="editMeal(index, 'snacks')" v-bind="props">
                    <v-card-text>{{ day.snacks[0].dish_name }}</v-card-text>
                </v-card>
                </template>
                <span>
                Ingredients: {{ day.snacks[0].ingredients }}<br>
                Calories: {{ day.snacks[0].calories }} kcal<br>
                Protein: {{ day.snacks[0].protein }}g<br>
                Carbs: {{ day.snacks[0].carbs }}g<br>
                Fats: {{ day.snacks[0].fats }}g
                </span>
            </v-tooltip>
            </td>
        </tr>
    </v-simple-table>
    </v-row>
</v-container>
</template>  

<script>
import OpenAI from "openai";

// Initialize OpenAI client
const openai = new OpenAI({
    apiKey: import.meta.env.VITE_OPENAI_API_KEY,
    dangerouslyAllowBrowser: true,
});

export default {
data() {
    return {
        mealPlan: [], // to store the structured meal plan
        isLoading: false, // loading status
    };
},
methods: {
    async generateMealPlan() {
    // Set loading to true while fetching
    this.isLoading = true;

    try {
        const currentDate = new Date();
        const formattedDate = `${currentDate.getDate().toString().padStart(2, '0')}/${(currentDate.getMonth() + 1).toString().padStart(2, '0')}`;

        // Call OpenAI API for chat completion
        const completion = await openai.chat.completions.create({
        model: "gpt-4o-mini",
        messages: [
            {
            role: "user",
            content: `
                Generate a 5-day meal plan starting from ${formattedDate}. Each day's plan should include breakfast, lunch, dinner, and snacks. 
                Your response must be in this JSON format and include no other content in your response:
                {
                "meal_plan": [
                    {
                        "date": "DD/MM",
                        "breakfast": [
                            {
                                "dish_name": "breakfast short description",
                                "ingredients": "main ingredients as one string separated by comma",
                                "calories": total calory of dish,
                                "protein": total calory of dish,
                                "carbs": total carbohydrates of dish,
                                "fats": total fat count of dish
                            }
                        ],
                        "lunch": [
                            {
                                "dish_name": "lunch dish short description",
                                "ingredients": "main ingredients as one string separated by comma",
                                "calories": total calory of dish,
                                "protein": total calory of dish,
                                "carbs": total carbohydrates of dish,
                                "fats": total fat count of dish
                            }
                        ],
                        "dinner": [
                            {
                                "dish_name": "dinner dish short description",
                                "ingredients": "main ingredients as one string separated by comma",
                                "calories": total calory of dish,
                                "protein": total calory of dish,
                                "carbs": total carbohydrates of dish,
                                "fats": total fat count of dish
                            }
                        ],
                        "snacks": [
                            {
                                "dish_name": "snack short description",
                                "ingredients": "main ingredients as one string separated by comma",
                                "calories": total calory of dish,
                                "protein": total calory of dish,
                                "carbs": total carbohydrates of dish,
                                "fats": total fat count of dish
                            }
                        ]
                    },
                    // Continue for all 5 days
                ]
                }
                
                Ensure that the 'date' field is in DD/MM format, and provide a healthy and varied meal for each day.
            `
            }
        ]
        });

        // Parse and store the meal plan content
        const mealPlanContent = JSON.parse(completion.choices[0].message.content.trim().replace(/```json|```/g, ''));
        this.mealPlan = mealPlanContent.meal_plan;

    } catch (error) {
        console.error("Error generating meal plan:", error);
        this.mealPlan = [];
    } finally {
        // Set loading to false after fetching is complete
        this.isLoading = false;
    }
    },
    async editMeal(dayIndex, mealType) {
    // Logic to allow editing or replacing a meal
    // For now, you can open a prompt to get a new meal name
    let newMeal = prompt(`Enter a new ${mealType} for ${this.mealPlan[dayIndex].date}`);
    if (newMeal) {
        try {
            // Call OpenAI API for chat completion
            const completion = await openai.chat.completions.create({
            model: "gpt-4o-mini",
            messages: [
                {
                role: "user",
                content: `Give me the following details as json for ${newMeal}. Response format:
                {
                    "dish_name": "${newMeal}",
                    "ingredients": "main ingredients as one string separated by comma",
                    "calories": total calory of dish,
                    "protein": total calory of dish,
                    "carbs": total carbohydrates of dish,
                    "fats": total fat count of dish
                }
                Include no other content but this json in your response.`
                }
            ]}
            )

            newMeal = JSON.parse(completion.choices[0].message.content.trim().replace(/```json|```/g, ''));
            this.mealPlan[dayIndex][mealType][0] = newMeal;

        } catch (error) {
            console.error("Error adding new meal:", error);
        } 
    }
    },
},
};
</script>

<style scoped>
/* Main container styles */
.main-container {
  background-color: #F7F8F3; /* Light cream background */
  padding: 20px;
  min-height: 100vh;
  text-align: center;
}

/* Title styles */
.title {
  color: #547045; /* Dark green */
  font-weight: bold;
}

.title-container {
    display: flex;
    justify-content: center;
    align-items: center;
    display: flex;
    margin-bottom: 20px;
}

/* Button styles */
.generate-button {
  margin-bottom: 20px;
  background-color: #87A37E !important; /* Greenish background */
  color: #FFFFFF !important; /* White text */
}

/* Meal plan row */
.meal-plan-row {
  margin-top: 20px;
  justify-content: center;
  align-items: center;
  display: flex;
}

/* Table cell for meal types */
.date-cell {
    font-weight: bold;
    color: black; /* Dark green */
}

/* Header Row for meal type */
.meal-header {
    font-weight: bold;
    color: white; 
    background-color: #547045; /* Darkish green */
}

/* Meal card styles */
.meal-card {
  background-color: #B0D1A8; /* Light green card background */
  color: #4A4A4A; /* Darker gray for text */
  padding: 10px;
  margin: 5px;
  cursor: pointer;
  border-radius: 8px;
  text-transform: capitalize;
}

/* Progress indicator */
.v-progress-circular {
  margin-top: 10px;
}

/* Button and loading container styles */
.button-container {
  display: flex;
  flex-direction: column; /* Stack button and loader vertically */
  align-items: center; /* Center them horizontally */
}

.food-tooltip {
    max-width: 1000px !important;
}
</style>
