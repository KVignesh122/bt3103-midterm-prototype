# NutriNUS Meal Plan Generator

NutriNUS is a Vue.js web application that helps university students plan healthy meals for their busy academic lifestyle. This interactive app generates personalized 5-day meal plans with healthy recipes, including details like ingredients, calories, proteins, carbohydrates, and fats. The app also allows users to update their meals as per their preferences.

<img src="https://github.com/user-attachments/assets/9d27486f-2d7b-499f-8ea7-94bcbe5108b7" alt="NutriNUS Meal Plan Generator" width="600" height="400"/>

## Features
- **5-Day Meal Plan Generation**: Automatically generate balanced meal plans for breakfast, lunch, dinner, and snacks.
- **Nutritional Information**: Hover over meal items to view detailed nutritional information including ingredients, calories, protein, carbs, and fats.
- **Interactive UI**: Simple and user-friendly interface to explore meal options and generate new meal plans.
- **Edit Meals**: Replace meals with new suggestions by simply clicking on the meal card.

## Technologies Used
- **Vue.js**: Frontend JavaScript framework for building the user interface.
- **Vuetify**: Material Design component framework for Vue.js to enhance UI components.
- **Vite**: Fast build tool and development server for Vue.js applications.
- **OpenAI API**: AI-powered meal plan generation and nutritional information.

## Getting Started

### Prerequisites
- **Node.js** and **npm** installed on your machine.

### Installation
1. **Clone the Repository**:
    ```bash
    git clone https://github.com/KVignesh122/bt3103-midterm-prototype.git
    cd bt3103-midterm-prototype
    ```
2. **Install Dependencies**:
    ```bash
    npm install
    ```

3. **Create a `.env` File**:
   - Add your OpenAI API key to the `.env` file in the root directory:
     ```
     VITE_OPENAI_API_KEY=your_openai_api_key_here
     ```

### Running the App
1. **Development Server**:
    ```bash
    npm run dev
    ```
2. Open the app in your browser:
   ```
   http://localhost:3000
   ```

## Deployment
To build and deploy the application, run:
```bash
npm run build
```
This will generate a production-ready build in the `dist` folder, ready for deployment to any web server or hosting service.

## Usage
1. **Generate a Meal Plan**: Click the "Generate" button to create a 5-day meal plan.
2. **View Nutrition Details**: Hover over any meal card to view detailed nutritional information.
3. **Edit a Meal**: Click on any meal card to replace it with a new meal suggestion based on your preferences.
