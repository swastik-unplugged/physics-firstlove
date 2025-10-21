import matplotlib.pyplot as plt
import numpy as np
from matplotlib.animation import FuncAnimation

def simulate_projectile_trajectory(initial_velocity, launch_angle, g=9.8):
    """
    Simulates the trajectory of a projectile and returns the time steps and positions.

    Args:
        initial_velocity (float): The initial velocity of the projectile in m/s.
        launch_angle (float): The launch angle in degrees.
        g (float): The acceleration due to gravity in m/s^2.

    Returns:
        tuple: A tuple containing arrays of time steps, x, and y coordinates.
    """
    # Convert launch angle to radians
    angle_rad = np.deg2rad(launch_angle)

    # Calculate initial velocity components
    vx_initial = initial_velocity * np.cos(angle_rad)
    vy_initial = initial_velocity * np.sin(angle_rad)

    # Calculate total flight time
    t_flight = 2 * vy_initial / g

    # Generate discrete time points for the simulation
    time_steps = np.linspace(0, t_flight, num=200)

    # Calculate x and y positions over time
    x_positions = vx_initial * time_steps
    y_positions = vy_initial * time_steps - 0.5 * g * time_steps**2

    # Ensure y positions do not go below zero
    y_positions[y_positions < 0] = 0

    return time_steps, x_positions, y_positions

# --- Simulation Parameters ---
projectile_velocity = 50  # Initial velocity in m/s
projectile_angle = 75     # Launch angle in degrees

# Calculate the trajectory
time_steps, x_coords, y_coords = simulate_projectile_trajectory(projectile_velocity, projectile_angle)

# --- Plotting and Visualization Setup ---
fig, ax = plt.subplots(figsize=(10, 6))

# Set plot limits and labels
ax.set_xlim(0, x_coords.max() * 1.1)
ax.set_ylim(0, y_coords.max() * 1.1)
ax.set_xlabel('Horizontal Distance (meters)', fontsize=12)
ax.set_ylabel('Vertical Height (meters)', fontsize=12)
ax.set_title(f'Animated Rocket Launch (Angle: {projectile_angle}°)', fontsize=14)
ax.grid(True)
plt.style.use('dark_background')

# Add a launchpad rectangle for visual effect
launchpad = plt.Rectangle((0, 0), 10, 10, color='gray', zorder=1)
ax.add_patch(launchpad)

# Create the trajectory line and the rocket point.
trajectory_line, = ax.plot([], [], 'o-', color='orange', markersize=2, label='Trajectory')
rocket_point, = ax.plot([], [], 'o', color='red', markersize=10, label='Rocket')

# --- Animation Functions ---
def init():
    """Initializes the animation by setting up empty plot data."""
    trajectory_line.set_data([], [])
    rocket_point.set_data([], [])
    return trajectory_line, rocket_point

def animate(frame):
    """Updates the plot for each frame of the animation."""
    # Set the new data for the trajectory line up to the current frame
    x_data_line = x_coords[:frame]
    y_data_line = y_coords[:frame]
    trajectory_line.set_data(x_data_line, y_data_line)

    # Set the new data for the rocket's current position.
    # The fix is to wrap the single float values in a list.
    x_data_point = x_coords[frame]
    y_data_point = y_coords[frame]
    rocket_point.set_data([x_data_point], [y_data_point])

    return trajectory_line, rocket_point

# --- Create and Run the Animation ---
# Store the FuncAnimation object to prevent it from being garbage-collected.
ani = FuncAnimation(
    fig,
    animate,
    frames=len(time_steps),
    init_func=init,
    blit=True,
    repeat=False
)

# Maximize the window on Windows
mng = plt.get_current_fig_manager()
mng.window.state('zoomed')

# Display the plot
plt.show()

