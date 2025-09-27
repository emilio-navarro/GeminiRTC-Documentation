# GeminiRTC - WebRTC Android Demo with Signaling Server

A production-ready WebRTC implementation for Android with Jetpack Compose UI and Node.js signaling server. This project demonstrates real-time peer-to-peer communication with comprehensive error handling, reactive state management, and modern Android architecture patterns.

## 📱 Project Overview
**🛑 NOTE:** This repository exists as a **read-only public portfolio piece** to showcase the architecture and technical depth of the private `GeminiRTC` codebase. **Contributions, Pull Requests, and Issues are not accepted for this specific document.  Contact me if you want to be a contributor.**

**GeminiRTC** is a complete WebRTC solution featuring:
- **Android Client**: Modern Jetpack Compose app with reactive WebRTC implementation
- **Signaling Server**: Node.js WebSocket server for peer coordination
- **Production Features**: Comprehensive error handling, logging, testing, and monitoring

## 🎥 Video Demonstration

Check out a live demo of the GeminiRTC application establishing a real-time data channel connection and exchanging messages.

[Download the video](https://github.com/emilio-navarro/GeminiRTC-Documentation/raw/refs/heads/main/geminirtc_demo.mp4)

## 🏗️ Architecture

### Android Architecture
- **UI Layer**: Jetpack Compose with Material 3 Design System
- **Navigation**: Compose Navigation with type-safe routing
- **State Management**: Single Source of Truth pattern with StateFlow
- **Dependency Injection**: Hilt for clean architecture and ViewModel injection
- **WebRTC Integration**: Stream WebRTC Android SDK with custom bridge pattern
- **Error Handling**: Production-grade error recovery and retry mechanisms  
- **Testing**: Comprehensive testing with Mockito, MockK, and Robolectric
- **Theming**: Custom Material 3 theme with extended color schemes
- **Modular Design**: Multi-module architecture with core/webrtc/app separation

### Signaling Server Architecture
- **WebSocket Server**: Node.js with session-aware message routing
- **Session Management**: Stateful offer/answer/ICE candidate handling
- **Error Recovery**: Automatic session reset and connection recovery
- **Memory Management**: Bounded buffers and leak prevention

## 🚀 Features

### Android Application
- ✅ **Real-time WebRTC Communication** via data channels
- ✅ **Modern Compose UI** with Material 3 Design System
- ✅ **Type-safe Navigation** with Compose Navigation Graph
- ✅ **Custom Composable Widgets** for WebRTC controls and monitoring
- ✅ **Multiple Channel Types**: Text, JSON, Audio Stream, Video Stream, File Transfer
- ✅ **Connection Quality Monitoring** with real-time statistics
- ✅ **Automatic Error Recovery** with exponential backoff retry
- ✅ **Comprehensive Logging** for debugging and monitoring
- ✅ **Dark/Light Theme Support** with extended color schemes
- ✅ **Connection State Visualization** with live channel status
- ✅ **Edge-to-Edge Design** with proper system bar handling
- ✅ **Splash Screen Integration** with Core SplashScreen API

### Signaling Server
- ✅ **Session-aware Message Routing** for reliable handshake coordination
- ✅ **Automatic Peer Discovery** and connection establishment
- ✅ **ICE Candidate Buffering** with size limits and cleanup
- ✅ **Connection Recovery** after server restarts or network issues
- ✅ **Health Check Endpoints** for monitoring and diagnostics

## 📋 Prerequisites

### Android Development
- **Android Studio**: 2024.2.1 (Ladybug) or newer
- **JDK**: 18 or higher
- **Android SDK**: API 33+ (minSdk: 33, targetSdk: 36)
- **Gradle**: 8.13.0+
- **Kotlin**: 2.2.20+

### Signaling Server
- **Node.js**: 16.0+ or higher
- **npm**: 8.0+ or higher
- **WebSocket Support**: Modern browser or WebSocket client

## 🛠️ Installation & Setup

### 1. Clone Repository
```bash
git clone https://github.com/emilio-navarro/GeminiRTC.git
cd GeminiRTC
```

### 2. Signaling Server Setup

#### Step-by-Step Installation:

1. **Navigate to Signal Server directory**:
   ```bash
   cd "Signal Server"
   ```

2. **Initialize Node.js project** (if not already done):
   ```bash
   npm init -y
   ```

3. **Install WebSocket dependency**:
   ```bash
   npm install ws
   ```

4. **Verify signaling-server.js**:
   ```bash
   node -c signaling-server.js
   ```

5. **Start the signaling server**:
   ```bash
   node signaling-server.js
   ```

6. **Verify server is running**:
   ```
   ✅ Output: WebRTC signaling server running on ws://10.0.0.202:8080
   ```

#### Server Configuration:
- **Default Port**: 8080
- **Host**: 10.0.0.202 (update IP in `signaling-server.js` for your network)
- **Protocol**: WebSocket (ws://)

### 3. Android Application Setup

#### Step-by-Step Installation:

1. **Navigate to Android directory**:
   ```bash
   cd Android
   ```

2. **Open in Android Studio**:
   ```bash
   # Or drag the Android folder into Android Studio
   studio .
   ```

3. **Update signaling server IP** (if needed):
   - Open `WebRTCBridgeImpl.kt`
   - Update the WebSocket URL to match your signaling server IP

4. **Sync Gradle dependencies**:
   - Android Studio will prompt to sync
   - Or manually: **File → Sync Project with Gradle Files**

5. **Build the project**:
   ```bash
   ./gradlew build
   ```

6. **Run on device/emulator**:
   - Connect Android device or start emulator
   - Click **Run** in Android Studio

## 🎨 Compose Architecture & UI Components

### Modern Compose Implementation
The GeminiRTC app showcases modern Android development using **Jetpack Compose** with a clean, modular architecture following Material 3 design principles.

#### Navigation Graph Structure
```kotlin
// Type-safe navigation with Compose Navigation
sealed class NavigationRoutes(val route: String) {
    data object Root : NavigationRoutes("root")
    data object WebRTC : NavigationRoutes("webrtc")
    data object Settings : NavigationRoutes("settings")
}

// NavGraph.kt - Centralized navigation with Hilt ViewModel injection
@Composable
fun NavGraph(
    navController: NavHostController,
    mainViewModel: MainViewModel,
    startDestination: String = NavigationRoutes.Root.route
) {
    NavHost(navController = navController, startDestination = startDestination) {
        composable(route = NavigationRoutes.WebRTC.route) {
            val webRTCViewModel: WebRTCViewModel = hiltViewModel()
            WebRTCScreen(viewModel = webRTCViewModel)
        }
    }
}
```

#### Custom Composable Widgets

##### 1. **ConnectionControls** - WebRTC Connection Management
```kotlin
@Composable
fun ConnectionControls(
    viewModel: IWebRTCViewModel,
    uiState: WebRTCUiState,
    isExpanded: Boolean = false
)
```
**Features:**
- 📱 **Collapsible UI** with expand/collapse animation
- 🌐 **Server URL Configuration** with validation
- 🎛️ **Initiator Mode Toggle** for offer/answer roles
- 🎨 **State-driven Button Design** with Material 3 styling
- ⚡ **Real-time Connection States** (Idle, Connecting, Connected, Error)

##### 2. **ChannelCommunication** - Real-time Message Interface  
```kotlin
@Composable
fun ChannelCommunication(
    viewModel: IWebRTCViewModel,
    uiState: WebRTCUiState
)
```
**Features:**
- 📋 **Tab-based Interface** with PrimaryTabRow
- 💬 **Multiple Channel Types** selection (Text, JSON, Audio, Video, File)
- 📝 **OutlinedTextField** for message input with validation
- 🚀 **Send Button** with loading states and error handling

##### 3. **ChannelState** - Live Connection Monitoring
```kotlin
@Composable  
fun ChannelState(viewModel: IWebRTCViewModel)
```
**Features:**
- 🔴 **Real-time State Indicators** (Open, Connecting, Closed, Failed)
- 📊 **LazyColumn** for efficient rendering of multiple channels
- 🎨 **Material Icons** for visual state representation
- ⚡ **StateFlow Integration** with automatic recomposition

##### 4. **ConnectionStatus** - Connection Quality Display
**Features:**
- 📈 **Connection Quality Metrics** with visual indicators  
- 🏓 **Latency Measurements** with real-time updates
- 📊 **WebRTC Statistics** display
- 🎯 **Connection Info** (Initiator status, peer details)

#### Material 3 Theme System

##### Extended Color Scheme
```kotlin
// Custom Material 3 extended colors
val MaterialTheme.extendedColorScheme: ExtendedColorScheme
    @Composable @ReadOnlyComposable
    get() = LocalExtendedColorScheme.current

// Usage in Composables
Card(
    colors = CardDefaults.cardColors(
        containerColor = MaterialTheme.extendedColorScheme.backgroundContent
    )
) {
    // Content with themed colors
}
```

##### Dynamic Theming
```kotlin
@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = false,
    fonts: AppFonts? = null,
    content: @Composable () -> Unit
)
```
**Features:**
- 🌙 **Dark/Light Mode** with system preference detection
- 🎨 **Dynamic Colors** (Android 12+ Material You)
- 🔤 **Custom Typography** with LocalAppFonts provider
- 🎯 **Extended Color Palette** beyond Material 3 defaults

#### Compose Preview Integration
```kotlin
// Comprehensive preview support with parameter providers
class ConnectionControlParameterProvider : PreviewParameterProvider<WebRTCUiState> {
    override val values: Sequence<WebRTCUiState> = sequenceOf(
        WebRTCUiState.Idle,
        WebRTCUiState.Connecting,
        WebRTCUiState.Connected(connectionInfo = ConnectionInfo()),
        WebRTCUiState.Error(errorType = WebRTCErrorType.NetworkError("Connection lost"))
    )
}

@Preview
@Preview(uiMode = Configuration.UI_MODE_NIGHT_YES)
@Composable
private fun PreviewConnectionControl(
    @PreviewParameter(ConnectionControlParameterProvider::class) uiState: WebRTCUiState
) {
    AppTheme {
        ConnectionControls(viewModel = MockWebRTCViewModel(), uiState = uiState)
    }
}
```

#### Edge-to-Edge Design
```kotlin
// MainActivity.kt - Modern system bar handling
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        enableEdgeToEdge() // Full screen with system bar transparency
        
        // Core SplashScreen API integration
        val splashScreen = installSplashScreen()
        splashScreen.setKeepOnScreenCondition { 
            splashRepository.isSplashScreenOn.value 
        }
        
        setContent {
            AppTheme {
                // Scaffold with proper system bar insets
                AppContent(/* parameters */)
            }
        }
    }
}
```

#### State Management with Compose
```kotlin
// Single Source of Truth pattern in ViewModels
class WebRTCViewModel : AndroidViewModel {
    // StateFlow for reactive UI updates
    private val _uiState = MutableStateFlow<WebRTCUiState>(WebRTCUiState.Idle)
    val uiState: StateFlow<WebRTCUiState> = _uiState.asStateFlow()
    
    // Channel-specific StateFlows 
    val channelStates: StateFlow<Map<DataChannelType, StateFlow<DataChannel.State>>>
    val channelMessages: StateFlow<Map<DataChannelType, StateFlow<MessageState>>>
}

// Composable observation with automatic recomposition
@Composable
fun WebRTCScreen(viewModel: IWebRTCViewModel) {
    val uiState by viewModel.uiState.collectAsState()
    
    // UI automatically updates when StateFlow emits new values
    when (uiState) {
        is WebRTCUiState.Connected -> ConnectedContent(uiState.connectionInfo)
        is WebRTCUiState.Error -> ErrorContent(uiState.errorType)
        // Other states...
    }
}
```

#### Latest Compose Libraries Integration

##### Core Compose Libraries (BOM: 2025.09.00)
- **compose-ui**: Core UI framework with declarative paradigm
- **compose-material3**: Material 3 design system implementation  
- **activity-compose**: Integration with ComponentActivity
- **lifecycle-viewmodel-compose**: ViewModel integration with Compose lifecycle
- **navigation-compose**: Type-safe navigation for Compose apps

##### Advanced Compose Features
- **compose-animation**: Smooth transitions and animations
- **compose-foundation-layout**: Advanced layout capabilities  
- **material-icons-extended**: Comprehensive icon library (1000+ icons)
- **ui-tooling-preview**: Enhanced preview system with parameters
- **lifecycle-runtime-compose**: Lifecycle-aware Compose integration
- **navigation-compose**: Type-safe navigation with animation support

#### Latest Compose Best Practices (2025)

##### 1. **State Hoisting Pattern**
```kotlin
// State is hoisted to parent composables for reusability
@Composable
fun ConnectionControls(
    serverUrl: String,
    onServerUrlChange: (String) -> Unit,
    isInitiator: Boolean,
    onInitiatorChange: (Boolean) -> Unit,
    uiState: WebRTCUiState,
    onConnect: () -> Unit
) {
    var isExpanded by remember { mutableStateOf(false) }
    
    // UI logic separated from state management
    ConnectionControlsContent(/* parameters */)
}
```

##### 2. **CompositionLocal Usage**
```kotlin
// Custom theme providers for consistent styling
val LocalExtendedColorScheme = staticCompositionLocalOf { extendedColorLight }
val LocalAppFonts = staticCompositionLocalOf<AppFonts> { error("No fonts provided") }

// Usage in composables
@Composable
fun ThemedText() {
    Text(
        text = "Styled Text",
        style = LocalAppFonts.current.titleMedium,
        color = MaterialTheme.extendedColorScheme.textPrimary
    )
}
```

##### 3. **Lifecycle-aware StateFlow Collection**
```kotlin
// Automatic lifecycle handling with collectAsStateWithLifecycle
@Composable
fun WebRTCScreen(viewModel: WebRTCViewModel) {
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()
    val channelStates by viewModel.channelStates.collectAsStateWithLifecycle()
    
    // UI automatically suspends/resumes with lifecycle
}
```

##### 4. **Preview Parameter Providers**
```kotlin
// Systematic preview testing with all possible states
class WebRTCUiStateProvider : PreviewParameterProvider<WebRTCUiState> {
    override val values: Sequence<WebRTCUiState> = sequenceOf(
        WebRTCUiState.Idle,
        WebRTCUiState.Connecting,
        WebRTCUiState.Connected(ConnectionInfo(connectionQuality = "Excellent")),
        WebRTCUiState.Disconnecting,
        WebRTCUiState.Loading(LoadingOperation.CONNECTING),
        WebRTCUiState.Error(WebRTCErrorType.NetworkError("Connection timeout"), isRetryable = true)
    )
}
```

##### 5. **Material 3 Theming Excellence**
```kotlin
// Extended color scheme beyond Material 3 defaults
@Immutable
data class ExtendedColorScheme(
    val backgroundScreen: Color,
    val backgroundContent: Color,
    val textPrimary: Color,
    val textSecondary: Color,
    val primary: Color,
    val error: Color,
    val success: Color,
    val warning: Color,
    val grey100: Color,
    val grey200: Color,
    val grey300: Color,
    val grey400: Color,
    val white: Color
) {
    companion object {
        fun lightDefault(): ExtendedColorScheme = ExtendedColorScheme(/* light colors */)
        fun darkDefault(): ExtendedColorScheme = ExtendedColorScheme(/* dark colors */)
    }
}
```

##### 6. **Compose Navigation Integration**
```kotlin
// Type-safe navigation with Hilt ViewModel injection
@Composable
fun NavGraph(navController: NavHostController) {
    NavHost(
        navController = navController,
        startDestination = NavigationRoutes.Root.route
    ) {
        composable(NavigationRoutes.WebRTC.route) {
            // Automatic ViewModel injection with Hilt
            val webRTCViewModel: WebRTCViewModel = hiltViewModel()
            WebRTCScreen(viewModel = webRTCViewModel)
        }
        
        composable(
            route = NavigationRoutes.Settings.route,
            enterTransition = { slideInHorizontally() },
            exitTransition = { slideOutHorizontally() }
        ) {
            SettingsScreen()
        }
    }
}
```

##### 7. **Edge-to-Edge System Integration**
```kotlin
// Modern system bar handling with Compose
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        enableEdgeToEdge() // Full screen experience
        
        setContent {
            AppTheme {
                Scaffold(
                    modifier = Modifier.fillMaxSize(),
                    containerColor = statusBarColor,
                    topBar = { /* Custom top bar */ },
                    bottomBar = { /* Navigation bar */ }
                ) { innerPadding ->
                    // Content respects system bar insets
                    NavGraph(modifier = Modifier.padding(innerPadding))
                }
            }
        }
    }
}
```

#### Modern Widget Architecture

##### **Reusable Composable Components**
```kotlin
// ScrollablePage - Custom layout composable
@Composable
fun ScrollablePage(
    headerContent: @Composable () -> Unit = {},
    bodyContent: @Composable () -> Unit
) {
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.spacedBy(AppDimensions.PaddingSmall)
    ) {
        item { headerContent() }
        item { bodyContent() }
    }
}

// Usage with WebRTC content
@Composable
fun WebRTCScreen(viewModel: IWebRTCViewModel) {
    ScrollablePage(
        headerContent = { /* Optional header */ },
        bodyContent = {
            Column {
                ConnectionControls(viewModel, uiState)
                ConnectionStatus(viewModel, uiState) 
                ChannelCommunication(viewModel, uiState)
                ChannelState(viewModel)
            }
        }
    )
}
```

## 📦 Dependencies

### Android Dependencies (Latest 2025 Versions)

#### **WebRTC & Real-time Communication**
```kotlin
// Stream WebRTC - Production-ready WebRTC SDK
implementation("io.getstream:stream-webrtc-android:1.1.3")
```

#### **Jetpack Compose & Modern UI (BOM 2025.09.00)**
```kotlin
// Compose BOM - Manages all Compose library versions
implementation(platform("androidx.compose:compose-bom:2025.09.00"))

// Core Compose UI
implementation("androidx.compose.ui:ui")
implementation("androidx.compose.ui:ui-graphics") 
implementation("androidx.compose.ui:ui-tooling-preview")

// Material 3 Design System
implementation("androidx.compose.material3:material3")
implementation("androidx.compose.material:material-icons-core")
implementation("androidx.compose.material:material-icons-extended")

// Compose Integration
implementation("androidx.activity:activity-compose:1.11.0")
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.9.4")
implementation("androidx.lifecycle:lifecycle-runtime-compose:2.9.4")

// Advanced Compose Features
implementation("androidx.compose.animation:animation")
implementation("androidx.compose.foundation:foundation-layout")
```

#### **Navigation & Architecture (Latest Versions)**
```kotlin
// Compose Navigation with type safety
implementation("androidx.navigation:navigation-compose:2.9.4")

// Dependency Injection with Hilt
implementation("com.google.dagger:hilt-android:2.57.1")
implementation("androidx.hilt:hilt-navigation-compose:1.3.0")
kapt("com.google.dagger:hilt-android-compiler:2.57.1")
kapt("androidx.hilt:hilt-compiler:1.3.0")

// AndroidX Lifecycle
implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.9.4")
implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.9.4")
implementation("androidx.lifecycle:lifecycle-viewmodel-savedstate:2.9.4")
```

#### **Core Android Libraries (2025 Latest)**
```kotlin
// AndroidX Core
implementation("androidx.core:core-ktx:1.17.0")
implementation("androidx.multidex:multidx:2.0.1") 
implementation("androidx.core:core-splashscreen:1.0.1")

// Window Management  
implementation("androidx.window:window:1.4.0")
```

#### **Kotlin & Coroutines (Latest 2025)**
```kotlin
// Kotlin Standard Library
implementation("org.jetbrains.kotlin:kotlin-stdlib:2.2.20")

// Coroutines for async programming
implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.10.2")

// Networking
implementation("com.squareup.okhttp3:okhttp:4.12.0")

// JSON Processing
implementation("com.google.code.gson:gson:2.13.2")

// Logging
implementation("com.jakewharton.timber:timber:5.0.1")
```

#### **Testing Framework (Comprehensive 2025)**
```kotlin
// Unit Testing
testImplementation("junit:junit:4.13.2")
testImplementation("org.mockito:mockito-core:5.5.0")
testImplementation("org.mockito.kotlin:mockito-kotlin:5.1.0")
testImplementation("io.mockk:mockk:1.13.8")

// Coroutines Testing
testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.10.2")

// Architecture Testing
testImplementation("androidx.arch.core:core-testing:2.2.0")

// Robolectric for Android unit testing
testImplementation("org.robolectric:robolectric:4.11.1")

// Instrumented Testing
androidTestImplementation("androidx.test.ext:junit:1.3.0")
androidTestImplementation("androidx.test.espresso:espresso-core:3.7.0")

// Compose Testing
androidTestImplementation(platform("androidx.compose:compose-bom:2025.09.00"))
androidTestImplementation("androidx.compose.ui:ui-test-junit4")
debugImplementation("androidx.compose.ui:ui-tooling")
debugImplementation("androidx.compose.ui:ui-test-manifest")
```

#### **Build Tools & Plugins (Latest)**
```kotlin
// Android Gradle Plugin 8.13.0
// Kotlin 2.2.20 with Compose Compiler 1.5.11  
// KtLint 11.5.1 for code formatting
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose)
    alias(libs.plugins.kapt)
    alias(libs.plugins.hilt)
    alias(libs.plugins.ktlint)
}
```

#### **Version Catalog (gradle/libs.versions.toml)**
```toml
[versions]
# Latest SDK versions (2025)
compileSdk = "36"
minSdk = "33"  
targetSdk = "36"
jvmTarget = "18"

# Latest library versions
compose-bom = "2025.09.00"
activity-compose = "1.11.0"
lifecycle = "2.9.4"
navigation = "2.9.4"
hilt = "2.57.1"
kotlin = "2.2.20"
coroutines = "1.10.2"
```

### Signaling Server Dependencies
```json
{
  "dependencies": {
    "ws": "^8.0.0"
  }
}
```

## 🏃‍♂️ Usage Guide

### Starting a WebRTC Connection

1. **Start Signaling Server**:
   ```bash
   cd "Signal Server"
   node signaling-server.js
   ```

2. **Launch Android Apps** (on two devices):
   - Install and run the app on two Android devices/emulators
   - Both devices should connect to the same signaling server

3. **Initiate Connection**:
   - Device A: Tap **"Start Connection"** (becomes initiator)
   - Device B: Tap **"Connect"** (becomes responder)
   - Connection should establish automatically

4. **Send Messages**:
   - Type text in the message field
   - Select channel type (Text, JSON, etc.)
   - Tap **"Send Message"**
   - Message appears on both devices

### Modular Architecture

#### Multi-Module Structure
```
Android/
├── app/                    # Main application module
│   ├── ui/screens/         # Compose screens and navigation
│   ├── viewmodels/         # Hilt ViewModels with StateFlow
│   └── nav/               # Navigation graph and routing
├── core/                   # Shared utilities and themes
│   ├── navigation/         # Navigation routes and providers
│   ├── resusables/         # Reusable Compose components
│   └── repositories/       # Data layer abstractions
└── webrtc/                # WebRTC implementation module
    ├── service/           # WebRTC bridge and implementation  
    ├── common/            # Data models and enums
    ├── error/             # Error handling framework
    └── logging/           # Structured logging system
```

#### Module Dependencies
- **app** → depends on **core** + **webrtc**
- **webrtc** → depends on **core** (shared utilities)
- **core** → standalone (base module)

### UI Components (Detailed Breakdown)

#### 1. **ConnectionControls Composable**
```kotlin
@Composable
fun ConnectionControls(
    viewModel: IWebRTCViewModel,
    uiState: WebRTCUiState,
    isExpanded: Boolean = false
)
```
**Interactive Features:**
- 🌐 **Server URL Configuration** with OutlinedTextField validation
- 🎛️ **Initiator Mode Toggle** (Checkbox for offer/answer role selection)
- 📱 **Collapsible Interface** with expand/collapse animation
- 🎯 **State-aware Button Logic**:
  - **Idle/Error**: Shows "Connect" button (enabled)
  - **Connecting**: Shows "Connecting..." with progress indicator (disabled)
  - **Connected**: Shows "Disconnect" with red styling
  - **Disconnecting**: Shows "Disconnecting..." with progress indicator

**Material 3 Integration:**
```kotlin
Card(
    colors = CardDefaults.cardColors(
        containerColor = MaterialTheme.extendedColorScheme.backgroundContent
    )
) {
    OutlinedTextField(
        colors = defaultOutlinedTextFieldColors(),
        enabled = uiState is WebRTCUiState.Idle || uiState is WebRTCUiState.Error
    )
}
```

#### 2. **ChannelCommunication Composable**
```kotlin  
@Composable
fun ChannelCommunication(
    viewModel: IWebRTCViewModel,
    uiState: WebRTCUiState
)
```
**Advanced Features:**
- 📋 **PrimaryTabRow** for channel type selection
- 💬 **Real-time Message Input** with validation and error states
- 🚀 **Smart Send Button** that adapts to connection state
- 📝 **Message History** display with ScrollablePage integration
- 🎨 **Channel-specific Styling** based on DataChannelType

**Tab Implementation:**
```kotlin
PrimaryTabRow(
    selectedTabIndex = selectedTabIndex,
    tabs = {
        DataChannelType.entries.filterNot { it == DataChannelType.Unknown }
            .forEachIndexed { index, channelType ->
                Tab(
                    selected = selectedTabIndex == index,
                    onClick = { selectedTabIndex = index },
                    text = { Text(channelType.id) }
                )
            }
    }
)
```

#### 3. **ChannelState Monitor**
```kotlin
@Composable  
fun ChannelState(viewModel: IWebRTCViewModel)
```
**Real-time State Visualization:**
- 🔴 **Live State Indicators** with Material Icons:
  - 🟢 `Icons.Default.CheckCircle` (OPEN) - Ready for communication
  - 🟡 `Icons.Default.Schedule` (CONNECTING) - Establishing connection  
  - 🔴 `Icons.Default.Close` (CLOSED) - Disconnected
  - ⚫ `Icons.Default.Block` (FAILED) - Connection failed

**StateFlow Integration:**
```kotlin
@Composable
fun ChannelState(viewModel: IWebRTCViewModel) {
    val channelStates by viewModel.channelStates.collectAsState()
    
    LazyColumn {
        items(channelStates.entries.toList()) { (type, stateFlow) ->
            val state by stateFlow.collectAsState()
            ChannelStateItem(channelType = type, state = state)
        }
    }
}
```

#### 4. **ConnectionStatus Display**
**Real-time Metrics:**
- 📈 **Connection Quality Visualization** with colored indicators
- 🏓 **Message Latency Tracking** with millisecond precision
- 📊 **WebRTC Statistics** (bytes sent/received, packet loss)
- 🎯 **Peer Connection Info** (initiator status, connection state)
- 📱 **Responsive Design** that adapts to screen sizes

#### 5. **Custom Theme Integration**
**Extended Color Scheme Usage:**
```kotlin
// Extended colors beyond Material 3 defaults
MaterialTheme.extendedColorScheme.backgroundScreen
MaterialTheme.extendedColorScheme.backgroundContent  
MaterialTheme.extendedColorScheme.textPrimary
MaterialTheme.extendedColorScheme.primary
MaterialTheme.extendedColorScheme.error
MaterialTheme.extendedColorScheme.grey300
```

#### 6. **Preview System Integration**
**Comprehensive Preview Coverage:**
```kotlin
// Parameter providers for different UI states
class ConnectionControlParameterProvider : PreviewParameterProvider<WebRTCUiState> {
    override val values: Sequence<WebRTCUiState> = sequenceOf(
        WebRTCUiState.Idle,
        WebRTCUiState.Connecting, 
        WebRTCUiState.Connected(ConnectionInfo(isInitiator = true)),
        WebRTCUiState.Error(WebRTCErrorType.NetworkError("Connection lost"))
    )
}

// Multi-configuration previews
@Preview
@Preview(uiMode = Configuration.UI_MODE_NIGHT_YES)
@Composable
private fun PreviewConnectionControl(
    @PreviewParameter(ConnectionControlParameterProvider::class) uiState: WebRTCUiState
) {
    AppTheme {
        ConnectionControls(MockWebRTCViewModel(), uiState)
    }
}
```

## 🧪 Testing

### Android Testing

#### Unit Tests
```bash
# Run all unit tests
./gradlew test

# Run specific test class
./gradlew test --tests "WebRTCViewModelTest"

# Run with coverage
./gradlew testDebugUnitTestCoverage
```

#### Instrumented Tests
```bash
# Run UI tests on device
./gradlew connectedAndroidTest

# Run specific UI test
./gradlew connectedAndroidTest -Pandroid.testInstrumentationRunnerArguments.class=WebRTCScreenTest
```

#### Testing Architecture
- **ViewModel Testing**: MockK for dependency mocking
- **Repository Testing**: Mockito for service layer testing  
- **UI Testing**: Compose testing framework
- **WebRTC Testing**: Mock WebRTC components for isolation

### Signaling Server Testing

#### Manual Testing
```bash
# Test server startup
node signaling-server.js

# Test WebSocket connection (using wscat if installed)
npm install -g wscat
wscat -c ws://10.0.0.202:8080
```

#### Connection Flow Testing
1. **Connect Client 1**: Should see "New peer connected"
2. **Send Offer**: JSON message with `type: "offer"`
3. **Connect Client 2**: Should receive buffered offer
4. **Send Answer**: JSON message with `type: "answer"`
5. **Exchange ICE**: Multiple ICE candidates between peers

## 🔧 Configuration

### Android Configuration

#### Update Signaling Server URL
```kotlin
// In WebRTCBridgeImpl.kt
private val signalingServerUrl = "ws://YOUR_SERVER_IP:8080"
```

#### Customize WebRTC Settings
```kotlin
// ICE servers configuration
private val iceServers = listOf(
    PeerConnection.IceServer.builder("stun:stun.l.google.com:19302").createIceServer()
)

// Data channel configuration  
private val dataChannelConfig = DataChannel.Init().apply {
    ordered = true
    maxRetransmits = 3
}
```

### Signaling Server Configuration

#### Change Server Port
```javascript
// In signaling-server.js
const server = new WebSocket.Server({ port: 8080 }); // Change port here
```

#### Update Host/IP Address
```javascript
// Update console log and any hardcoded references
console.log('WebRTC signaling server running on ws://YOUR_IP:8080');
```

#### Configure ICE Buffer Size
```javascript
// Maximum buffered ICE candidates
if (iceCandidatesBuffer.length > 50) { // Adjust limit
    iceCandidatesBuffer.shift();
}
```

## 🚨 Error Handling

### Android Error Recovery

The app includes comprehensive error handling via `WebRTCErrorHandler`:

#### Automatic Retry with Exponential Backoff
```kotlin
// Network errors: 5 retries with 2-second initial delay
// Signaling errors: 3 retries with 1-second initial delay
// Media errors: No retry (user intervention required)
```

#### Error Categories
- **NetworkError**: Connectivity issues (retryable)
- **SignalingError**: WebSocket/signaling failures (retryable)  
- **MediaError**: Camera/microphone access (non-retryable)
- **SecurityError**: Certificate/SSL issues (non-retryable)
- **ConfigurationError**: Invalid setup (non-retryable)
- **TimeoutError**: Operation timeouts (retryable)

#### Recovery Actions
- **RECONNECT**: Retry connection with same parameters
- **RESTART_SIGNALING**: Restart signaling channel
- **RESTART_MEDIA**: Restart media capture/playback
- **RESTART_CONNECTION**: Full connection restart
- **TERMINATE**: End connection (non-recoverable)

### Signaling Server Error Recovery

#### Session Reset Handling
- **Server Restart**: Sends reset signal to reconnecting clients
- **Stale ICE Candidates**: Rejects ICE without active session
- **Connection Cleanup**: Automatic peer removal on disconnect/error

#### Message Types
- **`reset`**: Server requests client to restart connection
- **`ping`**: Client health check request
- **`pong`**: Server responds with current state

## 📊 Logging & Monitoring

### Android Logging
```kotlin
// Structured logging with WebRTCLogger
WebRTCLogger.info("Connection established successfully")
WebRTCLogger.warn("Connection quality degraded", throwable = exception)
WebRTCLogger.error("Critical WebRTC failure", throwable = error)
```

#### Log Categories
- **Connection Events**: Peer connection state changes
- **Channel Events**: Data channel open/close/error
- **Message Events**: Send/receive with latency tracking
- **Error Events**: All error conditions with recovery actions

### Signaling Server Logging
```javascript
// Comprehensive server-side logging
console.log('New peer connected');
console.log('Stored new offer, cleared ICE buffer');
console.log('Buffered ICE candidate (X total)');
console.log('Session reset - server restarted');
```

## 🔒 Security Considerations

### Android Security
- **HTTPS/WSS**: Use secure WebSocket connections in production
- **Certificate Pinning**: Validate signaling server certificates
- **Permission Management**: Runtime camera/microphone permissions
- **Data Validation**: Sanitize all incoming WebRTC messages

### Signaling Server Security
- **Authentication**: Add user authentication for production use
- **Rate Limiting**: Prevent message flooding and DoS attacks
- **Input Validation**: Validate all incoming WebSocket messages
- **CORS Configuration**: Restrict cross-origin access appropriately

## 🐛 Troubleshooting

### Common Issues

#### Connection Fails to Establish
- ✅ **Check signaling server**: Ensure server is running and accessible
- ✅ **Verify IP address**: Update IP in Android app to match server
- ✅ **Check network**: Ensure devices can reach signaling server
- ✅ **Review firewall**: Allow WebSocket traffic on port 8080

#### UI Not Updating
- ✅ **StateFlow references**: Ensure ViewModel owns StateFlows
- ✅ **Compose observation**: Verify `collectAsState()` usage
- ✅ **Thread safety**: Check StateFlow updates on correct thread

#### ICE Connection Failures  
- ✅ **STUN servers**: Verify STUN server accessibility
- ✅ **NAT traversal**: May need TURN server for some networks
- ✅ **Firewall rules**: Allow UDP traffic for WebRTC

#### Server Restart Issues
- ✅ **Session reset**: App should handle reset messages from server
- ✅ **Reconnection logic**: Verify automatic reconnection works
- ✅ **State cleanup**: Ensure clean session state after disconnect

### Debug Commands

#### Android Debugging
```bash
# View app logs
adb logcat | grep "WebRTC"

# Check WebRTC internal logs
adb logcat | grep "org.webrtc"

# Monitor network connections
adb shell netstat | grep :8080
```

#### Server Debugging
```bash
# Check server process
ps aux | grep node

# Monitor WebSocket connections
netstat -an | grep :8080

# Check server logs
tail -f signaling-server.log  # if logging to file
```

## 📚 API Reference

### WebRTC Bridge Interface
```kotlin
interface WebRTCBridge {
    // Core WebRTC operations
    suspend fun createOffer(): SessionDescription
    suspend fun createAnswer(offer: SessionDescription): SessionDescription
    suspend fun setRemoteDescription(description: SessionDescription)
    
    // Data channel management
    suspend fun sendMessage(message: String, channelType: DataChannelType)
    fun setChannelStateFlows(
        channelStates: Map<DataChannelType, MutableStateFlow<DataChannel.State>>,
        channelMessages: Map<DataChannelType, MutableStateFlow<MessageState>>
    )
    
    // Connection management
    suspend fun connect(iceServers: List<PeerConnection.IceServer>): Boolean
    suspend fun disconnect()
    
    // State observation
    val connectionState: StateFlow<PeerConnection.PeerConnectionState>
    val iceConnectionState: StateFlow<PeerConnection.IceConnectionState>
    val connectionStats: StateFlow<WebRTCConnectionStats?>
    val connectionQuality: StateFlow<ConnectionQuality>
}
```

### Data Channel Types
```kotlin
sealed class DataChannelType(val id: String) {
    data object Text : DataChannelType("text")
    data object Json : DataChannelType("json")
    data object AudioStream : DataChannelType("audioStream")
    data object VideoStream : DataChannelType("videoStream")
    data object FileTransfer : DataChannelType("fileTransfer")
    data object Unknown : DataChannelType("unknown")
}
```

### Message States
```kotlin
sealed class MessageState {
    data object Loading : MessageState()
    data class Success(val message: DataChannelMessage) : MessageState()
    data class Error(val error: Throwable) : MessageState()
    data object Empty : MessageState()
}
```

### Signaling Server API
```javascript
// Message Types (JSON WebSocket messages)

// Offer message
{
  "type": "offer",
  "sdp": "v=0\r\no=- 123456789 2 IN IP4 127.0.0.1\r\n..."
}

// Answer message  
{
  "type": "answer", 
  "sdp": "v=0\r\no=- 987654321 2 IN IP4 127.0.0.1\r\n..."
}

// ICE candidate message
{
  "type": "ice",
  "candidate": "candidate:1 1 UDP 2130706431 192.168.1.100 54400 typ host",
  "sdpMLineIndex": 0,
  "sdpMid": "0",
  "id": "munay_peer"
}

// Server reset signal
{
  "type": "reset",
  "reason": "Server restarted, please reinitiate connection"  
}

// Health check
{
  "type": "ping"
}

// Server response
{
  "type": "pong",
  "sessionActive": true,
  "hasOffer": true,
  "hasAnswer": false,
  "bufferedCandidates": 3
}
```

## 🎯 Key Architecture Patterns

### Single Source of Truth Pattern
```kotlin
// ViewModel owns and manages all StateFlows
class WebRTCViewModel : AndroidViewModel {
    // Create StateFlows in ViewModel
    private val _channelStates = DataChannelType.entries
        .filter { it != DataChannelType.Unknown }
        .associateWith { MutableStateFlow(DataChannel.State.CONNECTING) }
    
    // Inject into WebRTCBridge
    init {
        webRTCBridge.setChannelStateFlows(_channelStates, _channelMessages)
    }
}
```

### Reactive UI with Compose
```kotlin
@Composable
fun ChannelState(viewModel: IWebRTCViewModel) {
    // Observe StateFlows in Composables
    val channelStates by viewModel.channelStates.collectAsState()
    
    // UI automatically recomposes when state changes
    LazyColumn {
        items(channelStates.entries.toList()) { (type, state) ->
            ChannelStateItem(channelType = type, state = state.collectAsState().value)
        }
    }
}
```

### Production Error Handling
```kotlin
// Categorized error handling with automatic recovery
suspend fun <T> executeWithRetry(
    operation: suspend () -> T,
    retryConfig: RetryConfig = RetryConfig(),
    onError: ((WebRTCError, Int) -> Unit)? = null
): Result<T> {
    // Exponential backoff retry logic
    // Categorized error handling
    // Automatic recovery strategies
}
```

## 🗺️ Roadmap & Future Development

### **Current Status: Production Ready v1.0**
GeminiRTC is a fully functional WebRTC demo showcasing modern Android development practices. The current implementation provides a solid foundation for real-time communication applications.

### **Planned Enhancements & Features**

#### **Phase 1: Core WebRTC Improvements** 🎯
- [ ] **Video Stream Support**
  - Camera capture integration with WebRTC
  - Video rendering in Compose UI
  - Camera switching (front/back)
  - Video quality controls

- [ ] **Audio Stream Enhancement**  
  - Microphone capture and playback
  - Audio quality indicators
  - Noise suppression and echo cancellation
  - Audio level visualization

- [ ] **File Transfer System**
  - Chunk-based file transfer over data channels
  - Progress indicators and cancellation
  - Multiple file format support
  - Transfer speed optimization

#### **Phase 2: Advanced Features** 🚀
- [ ] **Multi-Peer Support**
  - Room-based connections (multiple participants)
  - Peer discovery and management
  - Scalable signaling server architecture
  - Connection quality monitoring for multiple peers

- [ ] **Enhanced Security**
  - End-to-end encryption for data channels
  - Secure WebSocket connections (WSS)
  - Identity verification system
  - Certificate pinning implementation

- [ ] **UI/UX Enhancements**
  - Advanced Material 3 animations
  - Adaptive layouts for tablets and foldables
  - Accessibility improvements (TalkBack, large text)
  - Custom theme builder with user preferences

#### **Phase 3: Production Features** 💼
- [ ] **Analytics & Monitoring**
  - Connection quality metrics dashboard
  - Performance monitoring and alerts
  - User engagement analytics
  - Crash reporting integration

- [ ] **Cloud Integration**
  - TURN server integration for NAT traversal
  - Cloud-based signaling server deployment
  - WebRTC statistics collection and analysis
  - Scalable infrastructure setup

- [ ] **Developer Experience**
  - WebRTC SDK abstraction layer
  - Comprehensive API documentation
  - Integration examples and tutorials
  - Performance benchmarking tools

#### **Phase 4: Advanced Capabilities** ⚡
- [ ] **AI/ML Integration**
  - Real-time transcription of audio streams
  - Automatic language translation
  - Background noise filtering with AI
  - Smart connection optimization

- [ ] **Cross-Platform Expansion**
  - Flutter implementation for iOS/Android
  - Web client with WebRTC browser APIs
  - Desktop applications (Electron/Tauri)
  - React Native version

### **Research & Experimental Features** 🧪
- [ ] **WebRTC Innovations**
  - WebCodecs API integration
  - WebAssembly for performance optimization
  - Edge computing for low-latency connections
  - 5G network optimization

- [ ] **Emerging Technologies**
  - WebGPU for video processing
  - Machine learning on-device processing
  - Spatial audio for immersive experiences
  - AR/VR integration possibilities

## 🤝 Contributing

### **🔍 Exploration Welcome**
**Anyone is welcome to:**
- ⭐ **Star the repository** to show support
- 👀 **Explore the codebase** and learn from the implementation
- 📖 **Read the documentation** and understand the architecture
- 🐛 **Report bugs** through GitHub Issues
- 💡 **Suggest features** and improvements
- 📝 **Share feedback** on the implementation

### **🔐 Selective Contribution Process**

This project maintains high code quality and architectural integrity. To ensure the best possible contributions:

#### **Step 1: Request Access** 📧
**Before cloning or submitting PRs:**
1. **Email the maintainer**: [y2k_eclipse@hotmail.com](mailto:y2k_eclipse@hotmail.com)
2. **Include in your request**:
   - Your GitHub username
   - Brief description of your background/experience
   - Specific feature/area you'd like to contribute to
   - Expected timeline for your contribution

#### **Step 2: Approval Process** ✅
**Maintainer will evaluate based on:**
- Technical expertise alignment with project needs
- Quality of proposed contribution
- Commitment to project standards
- Available capacity for code review and mentorship

#### **Step 3: Authorized Development** 🛠️
**Once approved, you'll receive:**
- Repository access permissions
- Detailed contribution guidelines
- Code review process documentation
- Direct communication channel with maintainer

### **Development Workflow (Approved Contributors)**
1. **Fork** the repository (after approval)
2. **Create feature branch**: `git checkout -b feature/amazing-feature`
3. **Follow coding standards**: Ktlint, ESLint, proper documentation
4. **Include comprehensive tests**: Unit, UI, integration tests
5. **Submit PR** with detailed description and screenshots
6. **Code review**: Collaborative review with maintainer
7. **Merge**: After approval and CI/CD validation

### **Contribution Standards**

#### **Code Quality Requirements**
- **Kotlin**: Official conventions + ktlint compliance
- **Compose**: Material 3 best practices + performance optimization
- **JavaScript**: ESLint standard + comprehensive error handling
- **Documentation**: Clear comments for complex WebRTC logic
- **Testing**: 80%+ code coverage for new features

#### **Architectural Guidelines**
- **Single Responsibility**: Each component has one clear purpose
- **SOLID Principles**: Follow clean architecture patterns
- **State Management**: Use established StateFlow patterns
- **Error Handling**: Implement comprehensive error recovery
- **Performance**: Optimize for real-time communication requirements

### **Mentorship Program** 🎓
**For approved contributors:**
- **1:1 Technical Guidance**: Direct mentorship from project maintainer
- **Architecture Reviews**: Deep-dive sessions on WebRTC implementation
- **Code Quality Coaching**: Best practices for Android development
- **Career Development**: Networking and professional growth opportunities

### **Recognition & Rewards** 🏆
**Active contributors receive:**
- **Public Recognition**: Featured in project documentation
- **LinkedIn Recommendations**: Professional endorsements
- **Technical References**: Portfolio enhancement opportunities
- **Priority Access**: Early access to new features and updates

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Stream WebRTC**: Excellent WebRTC Android SDK (`io.getstream:stream-webrtc-android:1.1.3`)
- **Google WebRTC**: Core WebRTC implementation and standards
- **Jetpack Compose**: Modern declarative UI toolkit for Android
- **Material Design 3**: Beautiful and accessible design system
- **Hilt**: Powerful dependency injection framework
- **Kotlin Coroutines**: Excellent async programming support
## 📞 Support

### Getting Help
- **GitHub Issues**: [GitHub Issues](https://github.com/emilio-navarro/GeminiMQTT/issues)
- **Documentation**: Check this README for common solutions
- **Direct Contact**: Reach out to **Emilio Navarro** at [y2k_eclipse@hotmail.com](mailto:y2k_eclipse@hotmail.com)
- **LinkedIn**: Connect with **Emilio Navarro** on [LinkedIn](https://www.linkedin.com/in/emilionavarro/)

### Reporting Issues
When reporting issues, please include:
- **Environment**: Android version, device model, server setup
- **Steps to reproduce**: Detailed reproduction steps
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Logs**: Relevant Android logcat and server console output
- **Network setup**: Signaling server IP, firewall configuration

---

**Built with ❤️ for real-time communication and modern Android development**
