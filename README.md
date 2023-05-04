# C-code-on-topic-E-Vehicles
#include <stdio.h>
#include <stdbool.h>

#define MAX_BATTERY_CAPACITY 100 // Maximum battery capacity in kWh
#define MAX_DISTANCE_PER_CHARGE 300 // Maximum distance per charge in km
#define MAX_ACCELERATION 5 // Maximum acceleration in m/s^2
#define MAX_BRAKING 5 // Maximum braking in m/s^2
#define MAX_SPEED 150 // Maximum speed in km/h

// Define a struct to represent the vehicle's battery
typedef struct {
    float capacity; // Battery capacity in kWh
    float charge; // Current battery charge in kWh
} Battery;

// Define a struct to represent the vehicle
typedef struct {
    Battery battery;
    float speed; // Current speed in km/h
    float distance_travelled; // Distance travelled since last charge in km
} Vehicle;

// Function to charge the vehicle's battery
void charge_battery(Vehicle *vehicle) {
    printf("Charging battery...\n");
    // Simulate charging process
    vehicle->battery.charge = vehicle->battery.capacity;
    printf("Battery charged!\n");
}

// Function to drive the vehicle
void drive(Vehicle *vehicle, float acceleration, float braking, float time) {
    if (acceleration > MAX_ACCELERATION) {
        acceleration = MAX_ACCELERATION;
    }
    if (braking > MAX_BRAKING) {
        braking = MAX_BRAKING;
    }
    if (vehicle->speed >= MAX_SPEED) {
        vehicle->speed = MAX_SPEED;
    }
    // Calculate distance travelled based on acceleration and time
    float distance = vehicle->speed * time + 0.5 * acceleration * time * time;
    // Calculate new speed based on acceleration and time
    float new_speed = vehicle->speed + acceleration * time - braking * time;
    if (new_speed < 0) {
        new_speed = 0;
    }
    if (new_speed > MAX_SPEED) {
        new_speed = MAX_SPEED;
    }
    // Check if vehicle can travel the distance with current battery charge
    if (vehicle->battery.charge < distance * vehicle->battery.capacity / MAX_DISTANCE_PER_CHARGE) {
        printf("Not enough battery charge to travel that distance!\n");
        return;
    }
    // Update vehicle's distance travelled, speed and battery charge
    vehicle->distance_travelled += distance;
    vehicle->speed = new_speed;
    vehicle->battery.charge -= distance * vehicle->battery.capacity / MAX_DISTANCE_PER_CHARGE;
}

int main() {
    Vehicle vehicle = { { MAX_BATTERY_CAPACITY, 0 }, 0, 0 };
    // Prompt user to charge battery
    printf("Battery capacity: %f kWh\n", vehicle.battery.capacity);
    printf("Battery charge: %f kWh\n", vehicle.battery.charge);
    printf("Distance travelled: %f km\n", vehicle.distance_travelled);
    printf("Speed: %f km/h\n", vehicle.speed);
    printf("Do you want to charge the battery? (y/n): ");
    char c = getchar();
    if (c == 'y') {
        charge_battery(&vehicle);
    }
    // Prompt user to start driving
    printf("Start driving!\n");
    while (true) {
        // Get user input for acceleration and braking
        float acceleration, braking, time;
        printf("Acceleration (m/s^2): ");
        scanf("%f", &acceleration);
        printf("Braking (m/s^2): ");
        scanf("%f", &braking);
        printf("Time (s): ");
        scanf("%f", &time);
            // Drive the vehicle
    drive(&vehicle, acceleration, braking, time);
    // Print vehicle status
    printf("Battery charge: %f kWh\n", vehicle.battery.charge);
    printf("Distance travelled: %f km\n", vehicle.distance_travelled);
    printf("Speed: %f km/h\n", vehicle.speed);
    // Check if vehicle has run out of battery charge
    if (vehicle.battery.charge <= 0) {
        printf("Vehicle ran out of battery charge!\n");
        break;
    }
    // Check if vehicle has travelled the maximum distance per charge
    if (vehicle.distance_travelled >= MAX_DISTANCE_PER_CHARGE) {
        printf("Vehicle has travelled the maximum distance per charge!\n");
        break;
    }
}
    return 0;
}
