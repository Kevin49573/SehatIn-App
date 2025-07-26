<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SehatIn - AI Health Scanner</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <style>
        .gradient-bg { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .ai-pulse { animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
        .scan-animation { animation: scan 3s ease-in-out infinite; }
        @keyframes scan {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
        .floating { animation: float 6s ease-in-out infinite; }
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
        .logo-text {
            font-family: 'Inter', sans-serif;
            font-weight: 800;
            background: linear-gradient(135deg, #10b981, #3b82f6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .logo-icon {
            background: linear-gradient(135deg, #10b981, #3b82f6);
            border-radius: 12px;
            position: relative;
            overflow: hidden;
        }
        .logo-icon::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shine 3s infinite;
        }
        @keyframes shine {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }
    </style>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'primary-green': '#10b981',
                        'primary-blue': '#3b82f6',
                        'sehatin-teal': '#14b8a6'
                    }
                }
            }
        }
    </script>
</head>
<body class="min-h-screen bg-gradient-to-br from-emerald-50 via-white to-blue-50">
    <!-- Header -->
    <div class="bg-white shadow-sm border-b">
        <div class="max-w-6xl mx-auto px-4 py-4">
            <div class="flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="w-12 h-12 logo-icon flex items-center justify-center floating">
                        <svg class="w-7 h-7 text-white" fill="currentColor" viewBox="0 0 24 24">
                            <path d="M12 2C13.1 2 14 2.9 14 4C14 5.1 13.1 6 12 6C10.9 6 10 5.1 10 4C10 2.9 10.9 2 12 2ZM21 9V7L15 7V9C15 11.8 12.8 14 10 14S5 11.8 5 9V7L3 7V9C3 12.9 6.1 16 10 16V18H8V20H16V18H14V16C17.9 16 21 12.9 21 9Z"/>
                            <circle cx="12" cy="4" r="1" fill="rgba(255,255,255,0.9)"/>
                        </svg>
                    </div>
                    <div>
                        <h1 class="text-2xl font-bold logo-text">SehatIn</h1>
                        <div class="flex items-center gap-2">
                            <span class="text-sm text-gray-500">Powered by</span>
                            <span class="text-sm font-bold text-primary-blue">IBM Granite AI</span>
                            <div class="w-2 h-2 bg-primary-blue rounded-full ai-pulse"></div>
                        </div>
                    </div>
                </div>
                <div class="flex items-center gap-4">
                    <div class="flex items-center gap-2 px-4 py-2 bg-green-100 text-green-700 rounded-full text-sm font-medium">
                        <div class="w-2 h-2 bg-green-500 rounded-full ai-pulse"></div>
                        AI Connected
                    </div>
                    <div class="flex items-center gap-2 px-4 py-2 bg-blue-100 text-blue-700 rounded-full text-sm font-medium">
                        <i data-lucide="zap" size="16"></i>
                        Granite Ready
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <div class="max-w-6xl mx-auto px-4 py-6">
        <div class="flex flex-wrap gap-3 mb-8">
            <button id="scan-tab" class="tab-btn active flex items-center gap-2 px-6 py-3 rounded-xl font-medium transition-all">
                <i data-lucide="scan" size="20"></i>
                Health Scan
            </button>
            <button id="history-tab" class="tab-btn flex items-center gap-2 px-6 py-3 rounded-xl font-medium transition-all">
                <i data-lucide="bar-chart-3" size="20"></i>
                Health History
            </button>
            <button id="reports-tab" class="tab-btn flex items-center gap-2 px-6 py-3 rounded-xl font-medium transition-all">
                <i data-lucide="file-text" size="20"></i>
                AI Reports
            </button>
            <button id="profile-tab" class="tab-btn flex items-center gap-2 px-6 py-3 rounded-xl font-medium transition-all">
                <i data-lucide="user" size="20"></i>
                Profile
            </button>
        </div>

        <!-- Main Content -->
        <div class="tab-content">
            <!-- Scan Tab -->
            <div id="scan-content" class="tab-panel active">
                <div class="grid lg:grid-cols-2 gap-8">
                    <!-- Left Column - Scan Selection -->
                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <div class="flex items-center gap-3 mb-6">
                                <div class="w-8 h-8 bg-primary-blue rounded-lg flex items-center justify-center">
                                    <i data-lucide="brain" class="text-white" size="18"></i>
                                </div>
                                <h2 class="text-2xl font-bold text-gray-900">AI Analysis Selection</h2>
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <button class="scan-type-btn p-4 rounded-xl border-2 border-gray-200 hover:border-primary-green transition-all hover:scale-105" data-type="eye">
                                    <i data-lucide="eye" size="32" class="mx-auto mb-2 text-gray-600"></i>
                                    <h3 class="font-semibold text-gray-900">Eye Analysis</h3>
                                    <p class="text-sm text-gray-500 mt-1">Detect anemia, fatigue, liver issues</p>
                                </button>
                                <button class="scan-type-btn p-4 rounded-xl border-2 border-gray-200 hover:border-primary-green transition-all hover:scale-105" data-type="skin">
                                    <i data-lucide="hand" size="32" class="mx-auto mb-2 text-gray-600"></i>
                                    <h3 class="font-semibold text-gray-900">Skin Health</h3>
                                    <p class="text-sm text-gray-500 mt-1">Check hydration, vitamin levels</p>
                                </button>
                                <button class="scan-type-btn p-4 rounded-xl border-2 border-gray-200 hover:border-primary-green transition-all hover:scale-105" data-type="nail">
                                    <i data-lucide="hand" size="32" class="mx-auto mb-2 text-gray-600"></i>
                                    <h3 class="font-semibold text-gray-900">Nail Analysis</h3>
                                    <p class="text-sm text-gray-500 mt-1">Nutrient deficiency indicators</p>
                                </button>
                                <button class="scan-type-btn p-4 rounded-xl border-2 border-gray-200 hover:border-primary-green transition-all hover:scale-105" data-type="tongue">
                                    <i data-lucide="scan" size="32" class="mx-auto mb-2 text-gray-600"></i>
                                    <h3 class="font-semibold text-gray-900">Tongue Exam</h3>
                                    <p class="text-sm text-gray-500 mt-1">Digestive health, vitamin B12</p>
                                </button>
                            </div>
                        </div>

                        <!-- Image Upload -->
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Upload Medical Image</h3>
                            <div id="upload-area" class="border-2 border-dashed border-gray-300 rounded-xl p-8 text-center cursor-pointer hover:border-primary-green transition-colors">
                                <i data-lucide="upload" size="48" class="mx-auto text-gray-400 mb-4"></i>
                                <p class="text-gray-600 font-medium">Click to upload image or take photo</p>
                                <p class="text-sm text-gray-400 mt-2">Support: JPG, PNG, WebP • Max 10MB</p>
                                <div class="mt-4 flex justify-center gap-2">
                                    <span class="px-3 py-1 bg-blue-100 text-blue-700 text-xs rounded-full">AI Ready</span>
                                    <span class="px-3 py-1 bg-green-100 text-green-700 text-xs rounded-full">SehatIn Powered</span>
                                </div>
                            </div>
                            <input type="file" id="file-input" accept="image/*" capture="environment" class="hidden">
                        </div>

                        <!-- Scan Button -->
                        <button id="start-scan" class="w-full bg-gradient-to-r from-primary-green to-primary-blue text-white py-4 rounded-xl font-semibold text-lg hover:shadow-lg transition-all disabled:opacity-50 disabled:cursor-not-allowed">
                            <span class="scan-btn-text">Start SehatIn AI Analysis</span>
                        </button>
                    </div>

                    <!-- Right Column - Results -->
                    <div class="space-y-6">
                        <div id="results-container" class="hidden bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <!-- Results will be populated here -->
                        </div>

                        <!-- AI Features -->
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <div class="flex items-center gap-3 mb-6">
                                <div class="w-8 h-8 bg-sehatin-teal rounded-lg flex items-center justify-center">
                                    <i data-lucide="sparkles" class="text-white" size="18"></i>
                                </div>
                                <h3 class="text-lg font-semibold text-gray-900">SehatIn AI Features</h3>
                            </div>
                            <div class="space-y-4">
                                <div class="flex items-start gap-3 p-3 bg-blue-50 rounded-lg">
                                    <div class="w-8 h-8 bg-primary-blue rounded-lg flex items-center justify-center flex-shrink-0">
                                        <i data-lucide="brain" size="16" class="text-white"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-medium text-gray-900">Advanced Computer Vision</h4>
                                        <p class="text-sm text-gray-600">IBM Granite's state-of-the-art image analysis with medical knowledge base</p>
                                    </div>
                                </div>
                                <div class="flex items-start gap-3 p-3 bg-green-50 rounded-lg">
                                    <div class="w-8 h-8 bg-green-600 rounded-lg flex items-center justify-center flex-shrink-0">
                                        <i data-lucide="check-circle" size="16" class="text-white"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-medium text-gray-900">Non-Invasive Screening</h4>
                                        <p class="text-sm text-gray-600">Quick health assessment using smartphone camera technology</p>
                                    </div>
                                </div>
                                <div class="flex items-start gap-3 p-3 bg-teal-50 rounded-lg">
                                    <div class="w-8 h-8 bg-sehatin-teal rounded-lg flex items-center justify-center flex-shrink-0">
                                        <i data-lucide="trending-up" size="16" class="text-white"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-medium text-gray-900">Predictive Health Analytics</h4>
                                        <p class="text-sm text-gray-600">Track health trends and predict potential issues over time</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- History Tab -->
            <div id="history-content" class="tab-panel hidden">
                <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-900">Recent Scans</h3>
                            <span class="px-2 py-1 bg-blue-100 text-blue-700 text-xs rounded-full">12 Total</span>
                        </div>
                        <div class="space-y-3">
                            <div class="flex items-center gap-3 p-3 bg-gray-50 rounded-lg">
                                <i data-lucide="eye" class="text-blue-600" size="20"></i>
                                <div class="flex-1">
                                    <p class="font-medium text-sm">Eye Analysis</p>
                                    <p class="text-xs text-gray-500">2 hours ago</p>
                                </div>
                                <span class="px-2 py-1 bg-yellow-100 text-yellow-700 text-xs rounded-full">Medium</span>
                            </div>
                            <div class="flex items-center gap-3 p-3 bg-gray-50 rounded-lg">
                                <i data-lucide="hand" class="text-green-600" size="20"></i>
                                <div class="flex-1">
                                    <p class="font-medium text-sm">Skin Health</p>
                                    <p class="text-xs text-gray-500">1 day ago</p>
                                </div>
                                <span class="px-2 py-1 bg-green-100 text-green-700 text-xs rounded-full">Low</span>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                        <h3 class="font-semibold text-gray-900 mb-4">Health Trends</h3>
                        <div class="space-y-4">
                            <div>
                                <div class="flex justify-between mb-2">
                                    <span class="text-sm text-gray-600">Overall Health Score</span>
                                    <span class="text-sm font-bold text-green-600">85%</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2">
                                    <div class="bg-green-500 h-2 rounded-full" style="width: 85%"></div>
                                </div>
                            </div>
                            <div>
                                <div class="flex justify-between mb-2">
                                    <span class="text-sm text-gray-600">Risk Assessment</span>
                                    <span class="text-sm font-bold text-yellow-600">Medium</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2">
                                    <div class="bg-yellow-500 h-2 rounded-full" style="width: 60%"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                        <h3 class="font-semibold text-gray-900 mb-4">AI Insights</h3>
                        <div class="space-y-3">
                            <div class="p-3 bg-blue-50 rounded-lg">
                                <p class="text-sm text-blue-800">Your iron levels show improvement over the last week</p>
                            </div>
                            <div class="p-3 bg-green-50 rounded-lg">
                                <p class="text-sm text-green-800">Skin hydration has been consistently good</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Reports Tab -->
            <div id="reports-content" class="tab-panel hidden">
                <div class="grid lg:grid-cols-2 gap-8">
                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h2 class="text-xl font-bold text-gray-900 mb-6">Generate AI Report</h2>
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Report Type</label>
                                    <select class="w-full p-3 border border-gray-300 rounded-lg">
                                        <option>Weekly Health Summary</option>
                                        <option>Monthly Analysis</option>
                                        <option>Detailed Medical Report</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Date Range</label>
                                    <div class="grid grid-cols-2 gap-3">
                                        <input type="date" class="p-3 border border-gray-300 rounded-lg">
                                        <input type="date" class="p-3 border border-gray-300 rounded-lg">
                                    </div>
                                </div>
                                <button class="w-full bg-gradient-to-r from-primary-green to-primary-blue text-white py-3 rounded-lg font-medium hover:shadow-lg transition-all">
                                    Generate Report with SehatIn AI
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Recent Reports</h3>
                            <div class="space-y-3">
                                <div class="flex items-center gap-3 p-4 border border-gray-200 rounded-lg hover:bg-gray-50 cursor-pointer">
                                    <i data-lucide="file-text" class="text-blue-600" size="24"></i>
                                    <div class="flex-1">
                                        <p class="font-medium">Weekly Health Summary</p>
                                        <p class="text-sm text-gray-500">Generated 2 days ago</p>
                                    </div>
                                    <button class="px-3 py-1 bg-blue-100 text-blue-700 text-xs rounded-full hover:bg-blue-200">Download</button>
                                </div>
                                <div class="flex items-center gap-3 p-4 border border-gray-200 rounded-lg hover:bg-gray-50 cursor-pointer">
                                    <i data-lucide="file-text" class="text-green-600" size="24"></i>
                                    <div class="flex-1">
                                        <p class="font-medium">Monthly Analysis</p>
                                        <p class="text-sm text-gray-500">Generated 1 week ago</p>
                                    </div>
                                    <button class="px-3 py-1 bg-green-100 text-green-700 text-xs rounded-full hover:bg-green-200">Download</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Profile Tab -->
            <div id="profile-content" class="tab-panel hidden">
                <div class="grid lg:grid-cols-2 gap-8">
                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h2 class="text-xl font-bold text-gray-900 mb-6">Personal Information</h2>
                            <div class="space-y-4">
                                <div class="grid grid-cols-2 gap-4">
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">First Name</label>
                                        <input type="text" class="w-full p-3 border border-gray-300 rounded-lg" placeholder="Kevin">
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">Last Name</label>
                                        <input type="text" class="w-full p-3 border border-gray-300 rounded-lg" placeholder="Halim">
                                    </div>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Email</label>
                                    <input type="email" class="w-full p-3 border border-gray-300 rounded-lg" placeholder="kevinnatanielhalim@gmail.com">
                                </div>
                                <div class="grid grid-cols-2 gap-4">
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">Age</label>
                                        <input type="number" class="w-full p-3 border border-gray-300 rounded-lg" placeholder="18">
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">Gender</label>
                                        <select class="w-full p-3 border border-gray-300 rounded-lg">
                                            <option>Male</option>
                                            <option>Female</option>
                                            <option>Other</option>
                                        </select>
                                    </div>
                                </div>
                                <button class="w-full bg-gradient-to-r from-primary-green to-primary-blue text-white py-3 rounded-lg font-medium hover:shadow-lg transition-all">
                                    Save Profile
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">AI Preferences</h3>
                            <div class="space-y-4">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm font-medium text-gray-700">Enable AI Notifications</span>
                                    <input type="checkbox" class="toggle" checked>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-sm font-medium text-gray-700">Weekly Health Reports</span>
                                    <input type="checkbox" class="toggle" checked>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-sm font-medium text-gray-700">Share Data for AI Training</span>
                                    <input type="checkbox" class="toggle">
                                </div>
                            </div>
                        </div>
                        <div class="bg-white rounded-2xl p-6 shadow-lg border border-gray-100">
                            <h3 class="text-lg font-semibold text-gray-900 mb-4">Account Stats</h3>
                            <div class="grid grid-cols-2 gap-4">
                                <div class="text-center p-3 bg-blue-50 rounded-lg">
                                    <p class="text-2xl font-bold text-blue-600">81</p>
                                    <p class="text-sm text-gray-600">Total Scans</p>
                                </div>
                                <div class="text-center p-3 bg-green-50 rounded-lg">
                                    <p class="text-2xl font-bold text-green-600">47</p>
                                    <p class="text-sm text-gray-600">Reports Generated</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Initialize Lucide icons
        lucide.createIcons();

        // Global variables
        let selectedScanType = '';
        let uploadedImage = null;
        let isScanning = false;

        // Mock analysis data
        const mockAnalysis = {
            eye: {
                risk: 'medium',
                findings: ['Mild conjunctival pallor detected', 'Possible iron deficiency indicators', 'Slight redness in lower eyelid'],
                confidence: 78,
                recommendations: ['Consider iron-rich foods like spinach and red meat', 'Monitor energy levels throughout the day', 'Consult healthcare provider if symptoms persist', 'Get blood work done to check iron levels']
            },
            skin: {
                risk: 'low',
                findings: ['Normal skin hydration levels', 'Good elasticity indicators', 'Healthy color distribution'],
                confidence: 92,
                recommendations: ['Maintain current skincare routine', 'Continue adequate water intake (8-10 glasses daily)', 'Use sunscreen for UV protection']
            },
            nail: {
                risk: 'high',
                findings: ['Vertical ridges present', 'Pale nail beds observed', 'Possible B12 deficiency signs', 'Brittle nail texture detected'],
                confidence: 85,
                recommendations: ['Increase B12 rich foods (fish, eggs, dairy)', 'Consider B12 supplements after medical consultation', 'Schedule comprehensive blood test', 'Improve nail care routine']
            },
            tongue: {
                risk: 'medium',
                findings: ['Slight white coating present', 'Minor texture irregularities', 'Possible digestive concerns'],
                confidence: 73,
                recommendations: ['Improve oral hygiene routine', 'Monitor digestive symptoms', 'Stay adequately hydrated', 'Consider probiotic foods']
            }
        };

        // Tab functionality
        function initializeTabs() {
            const tabButtons = document.querySelectorAll('.tab-btn');
            const tabPanels = document.querySelectorAll('.tab-panel');

            tabButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const targetId = button.id.replace('-tab', '-content');
                    
                    // Remove active class from all tabs and panels
                    tabButtons.forEach(btn => btn.classList.remove('active'));
                    tabPanels.forEach(panel => panel.classList.add('hidden'));
                    
                    // Add active class to clicked tab and show corresponding panel
                    button.classList.add('active');
                    document.getElementById(targetId).classList.remove('hidden');
                });
            });
        }

        // Scan type selection
        function initializeScanTypes() {
            const scanTypeButtons = document.querySelectorAll('.scan-type-btn');
            
            scanTypeButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // Remove selection from all buttons
                    scanTypeButtons.forEach(btn => {
                        btn.classList.remove('border-primary-green', 'bg-green-50');
                        btn.classList.add('border-gray-200');
                        const icon = btn.querySelector('i');
                        icon.classList.remove('text-primary-green');
                        icon.classList.add('text-gray-600');
                    });
                    
                    // Add selection to clicked button
                    button.classList.remove('border-gray-200');
                    button.classList.add('border-primary-green', 'bg-green-50');
                    const icon = button.querySelector('i');
                    icon.classList.remove('text-gray-600');
                    icon.classList.add('text-primary-green');
                    
                    selectedScanType = button.dataset.type;
                    updateScanButton();
                });
            });
        }

        // File upload functionality
        function initializeFileUpload() {
            const uploadArea = document.getElementById('upload-area');
            const fileInput = document.getElementById('file-input');
            
            uploadArea.addEventListener('click', () => {
                fileInput.click();
            });
            
            fileInput.addEventListener('change', (event) => {
                const file = event.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        uploadedImage = e.target.result;
                        updateUploadArea();
                        updateScanButton();
                    };
                    reader.readAsDataURL(file);
                }
            });
            
            // Drag and drop functionality
            uploadArea.addEventListener('dragover', (e) => {
                e.preventDefault();
                uploadArea.classList.add('border-primary-green', 'bg-green-50');
            });
            
            uploadArea.addEventListener('dragleave', (e) => {
                e.preventDefault();
                uploadArea.classList.remove('border-primary-green', 'bg-green-50');
            });
            
            uploadArea.addEventListener('drop', (e) => {
                e.preventDefault();
                uploadArea.classList.remove('border-primary-green', 'bg-green-50');
                
                const files = e.dataTransfer.files;
                if (files.length > 0) {
                    const file = files[0];
                    if (file.type.startsWith('image/')) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            uploadedImage = e.target.result;
                            updateUploadArea();
                            updateScanButton();
                        };
                        reader.readAsDataURL(file);
                    }
                }
            });
        }

        function updateUploadArea() {
            const uploadArea = document.getElementById('upload-area');
            if (uploadedImage) {
                uploadArea.innerHTML = `
                    <img src="${uploadedImage}" alt="Uploaded" class="w-32 h-32 object-cover mx-auto rounded-lg mb-4 shadow-lg">
                    <p class="text-green-600 font-medium">Image uploaded successfully!</p>
                    <p class="text-sm text-gray-400 mt-2">Click to change image</p>
                    <div class="mt-4 flex justify-center gap-2">
                        <span class="px-3 py-1 bg-green-100 text-green-700 text-xs rounded-full">Ready for Analysis</span>
                        <span class="px-3 py-1 bg-blue-100 text-blue-700 text-xs rounded-full">SehatIn Powered</span>
                    </div>
                `;
            }
        }

        function updateScanButton() {
            const scanButton = document.getElementById('start-scan');
            const canScan = selectedScanType && uploadedImage && !isScanning;
            
            scanButton.disabled = !canScan;
            
            if (canScan) {
                scanButton.classList.remove('opacity-50', 'cursor-not-allowed');
            } else {
                scanButton.classList.add('opacity-50', 'cursor-not-allowed');
            }
        }

        // Scan functionality
        function initializeScan() {
            const scanButton = document.getElementById('start-scan');
            
            scanButton.addEventListener('click', () => {
                if (!selectedScanType || !uploadedImage || isScanning) return;
                
                startScan();
            });
        }

        function startScan() {
            isScanning = true;
            const scanButton = document.getElementById('start-scan');
            const scanBtnText = scanButton.querySelector('.scan-btn-text');
            
            // Update button to show scanning state
            scanBtnText.innerHTML = `
                <div class="flex items-center justify-center gap-2">
                    <div class="w-5 h-5 border-2 border-white border-t-transparent rounded-full animate-spin"></div>
                    Analyzing with SehatIn AI...
                </div>
            `;
            
            updateScanButton();
            
            // Simulate AI analysis
            setTimeout(() => {
                isScanning = false;
                scanBtnText.innerHTML = 'Start SehatIn AI Analysis';
                updateScanButton();
                showResults();
            }, 3000);
        }

        function showResults() {
            const resultsContainer = document.getElementById('results-container');
            const analysis = mockAnalysis[selectedScanType];
            
            const riskColors = {
                low: 'text-green-600 bg-green-50 border-green-200',
                medium: 'text-yellow-600 bg-yellow-50 border-yellow-200',
                high: 'text-red-600 bg-red-50 border-red-200'
            };
            
            const riskIcons = {
                low: 'check-circle',
                medium: 'clock',
                high: 'alert-triangle'
            };
            
            resultsContainer.innerHTML = `
                <div class="flex items-center justify-between mb-6">
                    <div class="flex items-center gap-3">
                        <div class="w-8 h-8 bg-primary-blue rounded-lg flex items-center justify-center">
                            <i data-lucide="brain" class="text-white" size="18"></i>
                        </div>
                        <h3 class="text-xl font-bold text-gray-900">SehatIn AI Analysis Results</h3>
                    </div>
                    <div class="flex items-center gap-2 px-4 py-2 rounded-full border ${riskColors[analysis.risk]}">
                        <i data-lucide="${riskIcons[analysis.risk]}" size="16"></i>
                        <span class="capitalize font-medium">${analysis.risk} Risk</span>
                    </div>
                </div>

                <!-- AI Confidence Score -->
                <div class="mb-6 p-4 bg-gradient-to-r from-green-50 to-blue-50 rounded-lg">
                    <div class="flex items-center justify-between mb-3">
                        <div class="flex items-center gap-2">
                            <i data-lucide="zap" size="16" class="text-primary-blue"></i>
                            <span class="text-sm font-medium text-gray-700">SehatIn AI Confidence</span>
                        </div>
                        <span class="text-lg font-bold text-gray-900">${analysis.confidence}%</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-3 shadow-inner">
                        <div class="bg-gradient-to-r from-primary-green to-primary-blue h-3 rounded-full transition-all duration-1000 shadow-sm" style="width: ${analysis.confidence}%"></div>
                    </div>
                    <p class="text-xs text-gray-500 mt-2">Based on advanced computer vision and medical knowledge base</p>
                </div>

                <!-- Key Findings -->
                <div class="mb-6">
                    <h4 class="font-semibold text-gray-900 mb-4 flex items-center gap-2">
                        <i data-lucide="search" size="16" class="text-primary-blue"></i>
                        Key Findings from AI Analysis
                    </h4>
                    <div class="space-y-3">
                        ${analysis.findings.map((finding, index) => `
                            <div class="flex items-start gap-3 p-4 bg-gray-50 rounded-lg border-l-4 border-primary-blue">
                                <div class="w-6 h-6 bg-primary-blue rounded-full flex items-center justify-center flex-shrink-0 mt-0.5">
                                    <span class="text-white text-xs font-bold">${index + 1}</span>
                                </div>
                                <span class="text-gray-700 leading-relaxed">${finding}</span>
                            </div>
                        `).join('')}
                    </div>
                </div>

                <!-- AI Recommendations -->
                <div class="mb-6">
                    <h4 class="font-semibold text-gray-900 mb-4 flex items-center gap-2">
                        <i data-lucide="lightbulb" size="16" class="text-sehatin-teal"></i>
                        SehatIn AI Recommendations
                    </h4>
                    <div class="space-y-3">
                        ${analysis.recommendations.map((rec, index) => `
                            <div class="flex items-start gap-3 p-4 bg-green-50 rounded-lg border-l-4 border-sehatin-teal">
                                <i data-lucide="check-circle" size="16" class="text-sehatin-teal mt-1 flex-shrink-0"></i>
                                <span class="text-gray-700 leading-relaxed">${rec}</span>
                            </div>
                        `).join('')}
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="grid grid-cols-2 gap-3 mb-6">
                    <button class="flex items-center justify-center gap-2 px-4 py-3 bg-primary-blue text-white rounded-lg hover:bg-blue-700 transition-all">
                        <i data-lucide="download" size="16"></i>
                        Download Report
                    </button>
                    <button class="flex items-center justify-center gap-2 px-4 py-3 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-all">
                        <i data-lucide="share" size="16"></i>
                        Share with Doctor
                    </button>
                </div>

                <!-- Medical Disclaimer -->
                <div class="p-4 bg-yellow-50 rounded-lg border border-yellow-200">
                    <div class="flex items-start gap-3">
                        <i data-lucide="alert-triangle" size="20" class="text-yellow-600 flex-shrink-0 mt-0.5"></i>
                        <div>
                            <p class="font-medium text-yellow-800 mb-2">Important Medical Disclaimer</p>
                            <p class="text-sm text-yellow-700 leading-relaxed">
                                This SehatIn AI analysis is for informational purposes only and should not replace professional medical advice, diagnosis, or treatment. Always consult with a qualified healthcare provider for proper medical assessment and care.
                            </p>
                        </div>
                    </div>
                </div>
            `;
            
            resultsContainer.classList.remove('hidden');
            lucide.createIcons();
        }

        // CSS for tab styling
        function addDynamicStyles() {
            const style = document.createElement('style');
            style.textContent = `
                .tab-btn.active {
                    background: linear-gradient(135deg, #10b981, #3b82f6);
                    color: white;
                    box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
                }
                .tab-btn:not(.active) {
                    color: #6b7280;
                }
                .tab-btn:not(.active):hover {
                    background-color: #f3f4f6;
                    color: #374151;
                }
                .toggle {
                    appearance: none;
                    width: 3rem;
                    height: 1.5rem;
                    background-color: #d1d5db;
                    border-radius: 9999px;
                    position: relative;
                    cursor: pointer;
                    transition: background-color 0.2s;
                }
                .toggle:checked {
                    background-color: #10b981;
                }
                .toggle::before {
                    content: '';
                    position: absolute;
                    top: 2px;
                    left: 2px;
                    width: 1.25rem;
                    height: 1.25rem;
                    background-color: white;
                    border-radius: 50%;
                    transition: transform 0.2s;
                }
                .toggle:checked::before {
                    transform: translateX(1.5rem);
                }
            `;
            document.head.appendChild(style);
        }

        // Initialize everything when DOM is loaded
        document.addEventListener('DOMContentLoaded', () => {
            addDynamicStyles();
            initializeTabs();
            initializeScanTypes();
            initializeFileUpload();
            initializeScan();
            
            // Add functionality to other interactive elements
            initializeInteractiveElements();
        });

        function initializeInteractiveElements() {
            // Add click handlers for download buttons in reports
            document.addEventListener('click', (e) => {
                if (e.target.closest('button') && e.target.textContent.includes('Download')) {
                    showNotification('Report downloaded successfully!', 'success');
                }
                
                if (e.target.closest('button') && e.target.textContent.includes('Generate Report')) {
                    showNotification('Generating report with SehatIn AI...', 'info');
                    setTimeout(() => {
                        showNotification('Report generated successfully!', 'success');
                    }, 2000);
                }
                
                if (e.target.closest('button') && e.target.textContent.includes('Save Profile')) {
                    showNotification('Profile saved successfully!', 'success');
                }
                
                if (e.target.closest('button') && e.target.textContent.includes('Share with Doctor')) {
                    showNotification('Report shared with healthcare provider!', 'success');
                }
            });
        }

        function showNotification(message, type = 'info') {
            const notification = document.createElement('div');
            const colors = {
                success: 'bg-green-500',
                error: 'bg-red-500',
                info: 'bg-blue-500',
                warning: 'bg-yellow-500'
            };
            
            notification.className = `fixed top-4 right-4 ${colors[type]} text-white px-6 py-3 rounded-lg shadow-lg z-50 transform translate-x-full transition-transform duration-300`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            // Slide in
            setTimeout(() => {
                notification.classList.remove('translate-x-full');
            }, 100);
            
            // Slide out and remove
            setTimeout(() => {
                notification.classList.add('translate-x-full');
                setTimeout(() => {
                    document.body.removeChild(notification);
                }, 300);
            }, 3000);
        }
    </script>
</body>
</html>
