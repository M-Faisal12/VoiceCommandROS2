# ROS2 Intelligent Assistant

A ROS2-based intelligent assistant system that uses speech recognition and AI-powered intent detection to manage schedules, notes, and daily planning tasks.

## Features

- **Speech Recognition**: Capture and convert voice input to text using Google's speech recognition API
- **Intent Detection**: Leverage advanced NLP with sentence transformers to intelligently classify user intents
- **Schedule Management**: Create and query scheduled events
- **Note Taking**: Create and retrieve notes
- **Daily Planning**: Generate and manage daily plans
- **ROS2 Architecture**: Built on ROS2 for scalable and modular robot applications

## Project Structure

```
ROS2/
├── src/
│   ├── my_py_pkg/                 # Main Python package
│   │   ├── my_py_pkg/
│   │   │   ├── listener.py        # Core listener node with speech & intent processing
│   │   │   ├── schedule.py        # Schedule service handler
│   │   │   ├── notes.py           # Notes service handler
│   │   │   ├── number_publisher.py
│   │   │   ├── number_counter.py
│   │   │   ├── reset_counter_client.py
│   │   │   └── my_first_node.py
│   │   ├── package.xml
│   │   └── setup.py
│   │
│   └── my_robot_interfaces/       # Custom ROS2 interfaces
│       ├── msg/
│       │   ├── Intent.msg         # Intent message (text, intent, confidence, action)
│       │   └── HardwareStatus.msg
│       ├── srv/
│       │   ├── Schedule.srv       # Schedule service
│       │   ├── Notes.srv          # Notes service
│       │   └── ResetCounter.srv
│       ├── package.xml
│       └── CMakeLists.txt
├── build/                          # Build artifacts
├── install/                        # Installed packages
├── Dockerfile
└── README.md
```

## Recognized Intents

The system recognizes the following user intents:

- `create_schedule` - Create a new scheduled event
- `query_schedule` - Query existing schedules
- `create_note` - Create a new note
- `query_notes` - Retrieve existing notes
- `query_daily_plan` - Get today's plan
- `unknown` - Intent not recognized

## Dependencies

### System Dependencies
- ROS2 (Humble or later)
- Python 3.8+

### Python Dependencies
- `rclpy` - ROS2 Python client library
- `sounddevice` - Audio recording
- `numpy` - Numerical computations
- `scipy` - Scientific computing
- `speech_recognition` - Speech-to-text conversion
- `sentence_transformers` - NLP and intent embeddings

## Installation

### 1. Prerequisites
Ensure you have ROS2 installed. If not, follow the [ROS2 installation guide](https://docs.ros.org/en/humble/Installation.html).

### 2. Clone the Repository
```bash
cd ~/ros2_ws/src
git clone <your-repo-url> ROS2
cd ~/ros2_ws
```

### 3. Install Dependencies
```bash
rosdep install -i --from-path src --rosdistro humble -y
```

### 4. Install Python Dependencies
```bash
pip install sounddevice numpy scipy speech_recognition sentence_transformers
```

### 5. Build the Workspace
```bash
colcon build
```

### 6. Source the Setup File
```bash
source install/setup.bash
```

## Usage

### Run the Listener Node
The main entry point that listens for voice input and processes intents:

```bash
ros2 run my_py_pkg listener
```

### Run Schedule Service
Start the schedule management service:

```bash
ros2 run my_py_pkg schedule
```

### Run Notes Service
Start the notes management service:

```bash
ros2 run my_py_pkg notes
```

## Node Details

### listener.py
Main node that:
- Records audio from the user
- Converts speech to text using Google Speech Recognition
- Extracts intent using sentence transformers
- Publishes Intent messages to other nodes
- Communicates with schedule and notes services

**Topics Published:**
- Intent messages for downstream processing

**Services Called:**
- `Schedule` service for schedule management
- `Notes` service for note management

### schedule.py
Service node for managing scheduled events:
- Creates new schedule entries
- Retrieves existing schedules
- Responds to schedule queries

### notes.py
Service node for managing notes:
- Creates new notes
- Retrieves existing notes
- Responds to note queries

## Custom Interfaces

### Intent Message (msg/Intent.msg)
```
string text          # The recognized text from speech
string intent        # Classified intent (e.g., "create_schedule")
float32 confidence   # Confidence score of the intent classification
string action        # Recommended action to take
```

### Schedule Service (srv/Schedule.srv)
```
Request:
  string request_type   # Type of request (create/query)
  string time          # Time for the event
  string event         # Event description

Response:
  string response      # Result of the operation
```

### Notes Service (srv/Notes.srv)
```
Request:
  string request_type  # Type of request (create/query)
  string note         # Note content

Response:
  string response     # Result of the operation
```

## Configuration

Audio recording parameters in `listener.py`:
- **Duration**: 5 seconds (configurable)
- **Sample Rate**: 44100 Hz
- **Channels**: 1 (mono)

Intent detection model:
- Uses `all-MiniLM-L6-v2` from Hugging Face (lightweight sentence transformer)

## Docker Support

A Dockerfile is provided for containerized deployment:

```bash
docker build -t ros2-assistant .
docker run -it ros2-assistant
```

## Troubleshooting

### No audio input detected
- Check that your microphone is properly connected and recognized by the system
- Verify permissions: `sudo usermod -a -G audio $USER`

### Speech recognition returns None
- Ensure internet connection (Google Speech API requires it)
- Check audio quality and background noise
- Verify microphone input levels

### Intent classification accuracy low
- Try reducing background noise
- Speak clearly and at a normal pace
- Adjust the confidence threshold if needed

## Future Enhancements

- [ ] Add multi-language support
- [ ] Implement local speech-to-text (offline capability)
- [ ] Add voice output for responses
- [ ] Create a web UI for schedule/notes management
- [ ] Implement persistent storage for schedules and notes
- [ ] Add advanced NLP for context-aware responses
- [ ] Support for multiple users with authentication
- [ ] Autonomous bot listening events and storing
- [ ] Hardware integration

## License

TODO: Add appropriate license

## Maintainer

M-Faisal (mfaisalsaeed18@gmail.com)

## Contributing

Contributions are welcome! Please ensure code follows ROS2 best practices and includes appropriate documentation.

## References

- [ROS2 Documentation](https://docs.ros.org/)
- [Sentence Transformers](https://www.sbert.net/)
- [SpeechRecognition Library](https://github.com/Uberi/speech_recognition)
