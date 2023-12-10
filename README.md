    #include <stdio.h>
    #include <stdbool.h>

    // Constants for commands
    #define START_ENGINE 1
    #define STOP_ENGINE 2
    #define INCREASE_TEMPERATURE 3
    #define DECREASE_TEMPERATURE 4
    #define DISPLAY_STATUS 5
    #define EXIT_PROGRAM 6

    // Structure to represent the car state
    struct CarState {
    bool engineRunning;
    int temperature;
    };

    // Function to start the engine
    void startEngine(struct CarState *car) {
      if (!car->engineRunning) {
        car->engineRunning = true;
        printf("Engine started.\n");
     } else {
        printf("Engine is already running.\n");
        }
    }

    // Function to stop the engine
    void stopEngine(struct CarState *car) {
    if (car->engineRunning) {
        car->engineRunning = false;
        printf("Engine stopped.\n");
    } else {
        printf("Engine is not running.\n");
      }
    }

    // Function to increase the temperature
    void increaseTemperature(struct CarState *car) {
       if (car->engineRunning) {
        car->temperature += 5;
        printf("Temperature increased. Current temperature: %d\n", car->temperature);
     } else {
        printf("Engine is not running. Cannot adjust temperature.\n");
       }
    }

    // Function to decrease the temperature
    void decreaseTemperature(struct CarState *car) {
      if (car->engineRunning) {
        car->temperature -= 5;
        printf("Temperature decreased. Current temperature: %d\n", car->temperature);
      } else {
        printf("Engine is not running. Cannot adjust temperature.\n");
     }
    }

    // Function to display car status
    void displayStatus(struct CarState *car) {
     printf("Car Status:\n");
     printf("Engine: %s\n", car->engineRunning ? "Running" : "Stopped");
     printf("Temperature: %d\n", car->temperature);
    }

    // Function to save car state to a file
    void saveCarStateToFile(struct CarState *car) {
      FILE *file = fopen("car_state.txt", "w");
      if (file == NULL) {
        perror("Error opening file");
      } else {
        fprintf(file, "Engine state(1:On, 2:Off): %d \n", car->engineRunning);
        fprintf(file, "Temperature: %dC",car->temperature);
        fclose(file);
        printf("Car state saved to car_state.txt.\n");
      }
    }

    // Function to load car state from a file
    void loadCarStateFromFile(struct CarState *car) {
      FILE *file = fopen("car_state.txt", "r");
      if (file == NULL) {
        perror("Error opening file");
      } else {
        fscanf(file, "%d %d", &car->engineRunning, &car->temperature);
        fclose(file);
        printf("Car state loaded from car_state.txt.\n");
      }
    }

    int main() {
      struct CarState car;

    // Load car state from file when starting the program
    loadCarStateFromFile(&car);

    int option;

    while (1) {
        printf("Choose an option:\n");
        printf("1. Start Engine\n");
        printf("2. Stop Engine\n");
        printf("3. Increase Temperature\n");
        printf("4. Decrease Temperature\n");
        printf("5. Display Car Status\n");
        printf("6. Exit\n");

        scanf("%d", &option);

        switch (option) {
            case START_ENGINE:
                startEngine(&car);
                break;

            case STOP_ENGINE:
                stopEngine(&car);
                break;

            case INCREASE_TEMPERATURE:
                increaseTemperature(&car);
                break;

            case DECREASE_TEMPERATURE:
                decreaseTemperature(&car);
                break;

            case DISPLAY_STATUS:
                displayStatus(&car);
                break;

            case EXIT_PROGRAM:
                printf("Exiting program.\n");
                // Save car state to file when exiting the program
                saveCarStateToFile(&car);
                return 0;

            default:
                printf("Invalid option. Please try again.\n");
                break;
        }
    }

    return 0;
}
