<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Graceful Giggles Babysitting - Book Your Session</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>

    <style>
        body {
            box-sizing: border-box;
        }
        
        .progress-fill {
            transition: width 0.3s ease;
        }
        
        .slide-enter {
            animation: slideIn 0.3s ease-out;
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }
        
        .time-slot {
            transition: all 0.2s ease;
        }
        
        .time-slot:hover {
            transform: translateY(-2px);
        }
        
        .time-slot.selected {
            background: linear-gradient(135deg, #18b79f, #16a085);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(24, 183, 159, 0.3);
        }
        
        .calendar-day {
            transition: all 0.2s ease;
            cursor: pointer;
        }
        
        .calendar-day:hover {
            background-color: #f0f9ff;
        }
        
        .calendar-day.selected {
            background: linear-gradient(135deg, #e88776, #d67765);
            color: white;
        }
        
        .calendar-day.disabled {
            opacity: 0.3;
            cursor: not-allowed;
        }
        
        .calendar-day.disabled:hover {
            background-color: transparent;
        }
        
        .calendar-day.check-in {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
            position: relative;
        }
        
        .calendar-day.check-out {
            background: linear-gradient(135deg, #3b82f6, #2563eb);
            color: white;
            position: relative;
        }
        
        .calendar-day.in-range {
            background: linear-gradient(135deg, #ddd6fe, #c4b5fd);
            color: #6b21a8;
            position: relative;
        }
        
        .calendar-day.check-in::after {
            content: "IN";
            position: absolute;
            bottom: 2px;
            right: 2px;
            font-size: 8px;
            font-weight: bold;
            opacity: 0.8;
        }
        
        .calendar-day.check-out::after {
            content: "OUT";
            position: absolute;
            bottom: 2px;
            right: 2px;
            font-size: 8px;
            font-weight: bold;
            opacity: 0.8;
        }
    </style>
</head>
<body style="background-color: #f8f0d1; font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;">
    <div class="min-h-screen py-4 px-4">
        <div class="max-w-2xl mx-auto">
            <!-- Progress Bar -->
            <div class="mb-8">
                <div class="bg-white rounded-full h-3 shadow-sm">
                    <div id="progressBar" class="progress-fill bg-gradient-to-r from-pink-400 to-teal-400 h-3 rounded-full" style="width: 20%;"></div>
                </div>
                <div class="mt-2 text-sm text-gray-600">
                    <span>Step <span id="currentStep">1</span> of 5</span>
                </div>
            </div>

            <!-- Slide 1: Enrollment Verification -->
            <div id="slide1" class="slide bg-white rounded-3xl shadow-lg p-8">
                <div class="text-center mb-8">
                    <div class="text-6xl mb-4">📅</div>
                    <h1 class="text-3xl font-bold mb-4" style="color: #e88776;">Book Your Babysitting Session! 🌟</h1>
                    <p class="text-gray-600 text-lg mb-6">Let's get your little ones booked for their next adventure! 🧸</p>
                </div>
                
                <div class="bg-gradient-to-r from-blue-50 to-teal-50 rounded-2xl p-6 mb-6">
                    <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">🔐 Enrollment Number Required</h3>
                    <p class="text-gray-700 mb-4">To book a session, you must have completed our enrollment process. Please enter your enrollment number below:</p>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Enrollment Number *</label>
                            <div class="relative">
                                <span class="absolute left-4 top-1/2 transform -translate-y-1/2 text-lg font-mono font-bold text-black z-10">GG-</span>
                                <input type="text" id="enrollmentNumber" placeholder="12345" class="w-full p-4 pl-16 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors text-center text-lg font-mono" maxlength="5">
                            </div>
                            <p class="text-xs text-gray-500 mt-1">Enter only the 5-digit number (e.g., 12345)</p>
                        </div>
                        
                        <button onclick="verifyEnrollment()" class="w-full py-4 px-6 rounded-2xl text-white font-semibold text-lg shadow-lg hover:shadow-xl transition-all" style="background-color: #18b79f;">
                            Verify & Continue 🔍
                        </button>
                    </div>
                </div>
                
                <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-xl">
                    <div class="flex items-start">
                        <div class="text-yellow-400 text-xl mr-3">⚠️</div>
                        <div>
                            <h4 class="font-semibold text-yellow-800">Don't have an enrollment number?</h4>
                            <p class="text-yellow-700 text-sm">You must complete our enrollment process first before booking. This ensures we have all necessary information to provide safe care for your children.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Slide 2: Family Information Display -->
            <div id="slide2" class="slide bg-white rounded-3xl shadow-lg p-8 hidden">
                <h2 class="text-2xl font-bold mb-6" style="color: #e88776;">Welcome Back! 👋</h2>
                
                <div id="familyInfo" class="bg-gradient-to-r from-green-50 to-blue-50 rounded-2xl p-6 mb-6">
                    <!-- Family information will be populated here -->
                </div>
                
                <div class="flex gap-4">
                    <button onclick="prevSlide()" class="flex-1 py-3 px-6 border-2 rounded-xl font-semibold transition-colors" style="border-color: #e88776; color: #e88776;">
                        ← Back
                    </button>
                    <button onclick="nextSlide()" class="flex-1 py-3 px-6 rounded-xl text-white font-semibold transition-colors" style="background-color: #18b79f;">
                        Continue to Booking →
                    </button>
                </div>
            </div>

            <!-- Slide 3: Date & Time Selection -->
            <div id="slide3" class="slide bg-white rounded-3xl shadow-lg p-8 hidden">
                <h2 class="text-2xl font-bold mb-6" style="color: #e88776;">Select Dates & Times 📅</h2>
                
                <div class="space-y-6">
                    <!-- Booking Mode Selection -->
                    <div class="bg-blue-50 rounded-2xl p-4 space-y-3">
                        <h4 class="font-medium text-gray-800">Booking Type</h4>
                        <div class="space-y-2">
                            <label class="flex items-center gap-3 cursor-pointer">
                                <input type="radio" name="bookingMode" value="single" onchange="changeBookingMode('single')" class="text-teal-600" checked>
                                <span class="font-medium text-gray-800">⏰ Single Session</span>
                            </label>
                            <p class="text-sm text-gray-600 ml-6">Book one babysitting session</p>
                            
                            <label class="flex items-center gap-3 cursor-pointer">
                                <input type="radio" name="bookingMode" value="multiple" onchange="changeBookingMode('multiple')" class="text-teal-600">
                                <span class="font-medium text-gray-800">📋 Multiple Sessions</span>
                            </label>
                            <p class="text-sm text-gray-600 ml-6">Book several separate sessions at once</p>
                            
                            <label class="flex items-center gap-3 cursor-pointer">
                                <input type="radio" name="bookingMode" value="multiday" onchange="changeBookingMode('multiday')" class="text-teal-600">
                                <span class="font-medium text-gray-800">🌙 Multi-Day Sitting</span>
                            </label>
                            <p class="text-sm text-gray-600 ml-6">Consecutive days with overnight care (2-7 days)</p>
                        </div>
                    </div>
                    
                    <!-- Calendar -->
                    <div>
                        <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">
                            <span id="dateSelectionTitle">Choose Your Date</span>
                            <span id="selectedDatesCount" class="text-sm font-normal text-gray-600 ml-2"></span>
                        </h3>
                        <div class="bg-gray-50 rounded-2xl p-6">
                            <div class="flex justify-between items-center mb-4">
                                <button onclick="changeMonth(-1)" class="p-2 rounded-lg hover:bg-gray-200 transition-colors">
                                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
                                    </svg>
                                </button>
                                <h4 id="currentMonth" class="text-xl font-semibold"></h4>
                                <button onclick="changeMonth(1)" class="p-2 rounded-lg hover:bg-gray-200 transition-colors">
                                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                                    </svg>
                                </button>
                            </div>
                            
                            <div class="grid grid-cols-7 gap-2 mb-2">
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Sun</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Mon</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Tue</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Wed</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Thu</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Fri</div>
                                <div class="text-center text-sm font-medium text-gray-500 py-2">Sat</div>
                            </div>
                            
                            <div id="calendarGrid" class="grid grid-cols-7 gap-2">
                                <!-- Calendar days will be populated here -->
                            </div>
                            
                            <div id="clearDatesBtn" class="mt-4 hidden">
                                <button onclick="clearSelectedDates()" class="px-4 py-2 bg-red-100 text-red-700 rounded-lg hover:bg-red-200 transition-colors text-sm">
                                    Clear All Selected Dates
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Multi-Day Stay Configuration -->
                    <div id="multiDayConfig" class="hidden">
                        <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">Multi-Day Stay Details</h3>
                        <div class="bg-gradient-to-r from-purple-50 to-pink-50 rounded-2xl p-6 space-y-4">
                            <div class="bg-white rounded-xl p-4 border-2 border-purple-200">
                                <h4 class="font-medium text-gray-800 mb-3">📅 Select Your Sitting Period</h4>
                                <p class="text-sm text-gray-600 mb-3">Click on the calendar above to select your <strong>start date</strong>, then click your <strong>end date</strong>. The dates in between will be automatically highlighted.</p>
                                
                                <div id="multiDaySelection" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    <div class="text-center p-3 bg-green-50 rounded-lg border border-green-200">
                                        <div class="text-sm font-medium text-green-800">Start Date</div>
                                        <div id="checkInDisplay" class="text-lg font-bold text-green-700">Not selected</div>
                                    </div>
                                    <div class="text-center p-3 bg-blue-50 rounded-lg border border-blue-200">
                                        <div class="text-sm font-medium text-blue-800">End Date</div>
                                        <div id="checkOutDisplay" class="text-lg font-bold text-blue-700">Not selected</div>
                                    </div>
                                </div>
                                
                                <div id="multiDayPreview" class="hidden mt-4">
                                    <div class="bg-gradient-to-r from-green-50 to-blue-50 rounded-lg p-4 border border-gray-200">
                                        <h4 class="font-medium text-gray-800 mb-2">Sitting Overview:</h4>
                                        <div id="multiDayDates" class="text-sm text-gray-600">
                                            <!-- Dates will be populated here -->
                                        </div>
                                    </div>
                                </div>
                                
                                <button id="clearMultiDayDates" onclick="clearMultiDaySelection()" class="mt-3 px-4 py-2 bg-red-100 text-red-700 rounded-lg hover:bg-red-200 transition-colors text-sm hidden">
                                    Clear Selection
                                </button>
                            </div>
                            
                            <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-lg">
                                <div class="flex items-start">
                                    <div class="text-yellow-400 text-lg mr-2">🌙</div>
                                    <div class="text-sm text-yellow-700">
                                        <strong>Multi-Day Sitting includes:</strong><br>
                                        • 24/7 care during the entire period<br>
                                        • Overnight supervision<br>
                                        • Meals and daily activities<br>
                                        • Bedtime routines and morning care<br>
                                        • Emergency contact availability
                                    </div>
                                </div>
                            </div>
                            
                            <!-- Child Selection for Multi-Day -->
                            <div id="multiDayChildSelection" class="mt-6 hidden">
                                <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">Which Children? *</h3>
                                <div class="space-y-3" id="multiDayChildrenList">
                                    <!-- Children checkboxes will be populated here -->
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Selected Dates List -->
                    <div id="selectedDatesList" class="hidden">
                        <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">Selected Sessions</h3>
                        <div id="selectedDatesContainer" class="space-y-3">
                            <!-- Selected dates will be populated here -->
                        </div>
                    </div>
                    
                    <!-- Time Slots (for single date mode) -->
                    <div id="singleDateTimeSelection">
                        <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">Start Time of Session</h3>
                        <div class="grid grid-cols-3 md:grid-cols-4 gap-3" id="timeSlots">
                            <!-- Time slots will be populated here -->
                        </div>
                        
                        <h3 class="text-lg font-semibold mb-4 mt-6" style="color: #18b79f;">Session Duration</h3>
                        <div class="grid grid-cols-2 md:grid-cols-5 gap-3">
                            <button onclick="selectDuration(1)" class="duration-btn p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors">
                                <div class="font-semibold">1 Hour</div>
                            </button>
                            <button onclick="selectDuration(2)" class="duration-btn p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors">
                                <div class="font-semibold">2 Hours</div>
                            </button>
                            <button onclick="selectDuration(3)" class="duration-btn p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors">
                                <div class="font-semibold">3 Hours</div>
                            </button>
                            <button onclick="selectDuration(4)" class="duration-btn p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors">
                                <div class="font-semibold">4 Hours</div>
                            </button>
                            <button onclick="selectDuration('flexible')" class="duration-btn p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors">
                                <div class="font-semibold">Don't Know</div>
                                <div class="text-sm text-gray-600">Flexible</div>
                            </button>
                        </div>
                        
                        <!-- Child Selection for Single Date -->
                        <div id="singleChildSelection" class="mt-6 hidden">
                            <h3 class="text-lg font-semibold mb-4" style="color: #18b79f;">Which Children? *</h3>
                            <div class="space-y-3" id="singleChildrenList">
                                <!-- Children checkboxes will be populated here -->
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="flex gap-4 mt-8">
                    <button onclick="prevSlide()" class="flex-1 py-3 px-6 border-2 rounded-xl font-semibold transition-colors" style="border-color: #e88776; color: #e88776;">
                        ← Back
                    </button>
                    <button onclick="nextSlide()" class="flex-1 py-3 px-6 rounded-xl text-white font-semibold transition-colors" style="background-color: #18b79f;">
                        Continue →
                    </button>
                </div>
            </div>

            <!-- Slide 4: Additional Details -->
            <div id="slide4" class="slide bg-white rounded-3xl shadow-lg p-8 hidden">
                <h2 class="text-2xl font-bold mb-6" style="color: #e88776;">Session Details 📝</h2>
                
                <div class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Session Location *</label>
                        <div class="space-y-3">
                            <label class="flex items-center gap-3 p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors cursor-pointer">
                                <input type="radio" name="location" value="home" required class="text-teal-600">
                                <div>
                                    <div class="font-semibold">🏠 At Your Home</div>
                                    <div class="text-sm text-gray-600">Our babysitter comes to your address</div>
                                </div>
                            </label>
                            <label class="flex items-center gap-3 p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors cursor-pointer">
                                <input type="radio" name="location" value="public" required class="text-teal-600">
                                <div>
                                    <div class="font-semibold">🌳 Public Location</div>
                                    <div class="text-sm text-gray-600">Park, playground, or other public space</div>
                                </div>
                            </label>
                        </div>
                    </div>
                    
                    <div id="publicLocationDetails" class="hidden">
                        <label class="block text-sm font-medium text-gray-700 mb-2">Specify Public Location *</label>
                        <input type="text" id="publicLocationInput" class="w-full p-4 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="e.g., Gzira Garden, Sliema Promenade">
                    </div>
                    
                    <!-- Pickup & Drop-off Options -->
                    <div id="transportSection">
                        <label class="block text-sm font-medium text-gray-700 mb-2">Pickup & Drop-off Services 🚗</label>
                        <div class="bg-blue-50 rounded-xl p-4 mb-3">
                            <p class="text-sm text-blue-700 mb-2">Need transportation help? We can pick up or drop off your child!</p>
                            <p class="text-xs text-blue-600">🚗 Available for sessions of 3+ hours only</p>
                        </div>
                        
                        <div id="transportOptions">
                            <!-- Transport options will be populated based on booking mode -->
                        </div>
                        
                        <div class="mt-3 p-3 bg-yellow-50 border-l-4 border-yellow-400 rounded-r-lg">
                            <div class="flex items-start">
                                <div class="text-yellow-400 text-lg mr-2">💡</div>
                                <div class="text-sm text-yellow-700">
                                    <strong>Popular scenarios:</strong><br>
                                    • Pick up from school → Babysit at home<br>
                                    • Babysit at home → Drop off at activity<br>
                                    • Pick up from school → Babysit → Drop off at home
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Special Instructions</label>
                        <textarea id="specialInstructions" rows="4" class="w-full p-4 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="Any specific activities, routines, or instructions for the babysitter..."></textarea>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Emergency Contact During Session</label>
                        <div class="bg-blue-50 rounded-xl p-4">
                            <p class="text-sm text-blue-700 mb-3">We'll use your primary emergency contact from enrollment. Want to use a different contact for this session?</p>
                            <label class="flex items-center gap-2">
                                <input type="checkbox" id="useAlternateContact" onchange="toggleAlternateContact()" class="text-teal-600">
                                <span class="text-sm">Use different emergency contact for this session</span>
                            </label>
                        </div>
                        
                        <div id="alternateContactDetails" class="mt-4 space-y-3 hidden">
                            <input type="text" id="alternateContactName" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="Contact Name">
                            <input type="tel" id="alternateContactPhone" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="Phone Number">
                        </div>
                    </div>
                </div>
                
                <div class="flex gap-4 mt-8">
                    <button onclick="prevSlide()" class="flex-1 py-3 px-6 border-2 rounded-xl font-semibold transition-colors" style="border-color: #e88776; color: #e88776;">
                        ← Back
                    </button>
                    <button onclick="nextSlide()" class="flex-1 py-3 px-6 rounded-xl text-white font-semibold transition-colors" style="background-color: #18b79f;">
                        Review Booking →
                    </button>
                </div>
            </div>

            <!-- Slide 5: Booking Summary -->
            <div id="slide5" class="slide bg-white rounded-3xl shadow-lg p-8 hidden">
                <h2 class="text-2xl font-bold mb-6" style="color: #e88776;">Booking Summary 📋</h2>
                
                <div id="bookingSummary" class="space-y-6 mb-8">
                    <!-- Booking summary will be populated here -->
                </div>
                
                <div class="bg-gradient-to-r from-yellow-50 to-orange-50 rounded-2xl p-6 mb-6">
                    <h3 class="text-lg font-semibold mb-3" style="color: #e88776;">💳 Payment Information</h3>
                    <div class="space-y-2 text-sm text-gray-700">
                        <p><strong>Payment Details:</strong> Will be discussed after confirmation</p>
                        <p><strong>Payment Due:</strong> After session completion</p>
                        <p><strong>Cancellation:</strong> Must cancel 24+ hours in advance</p>
                    </div>
                </div>
                
                <div class="mb-6">
                    <label class="flex items-start gap-3 cursor-pointer">
                        <input type="checkbox" id="bookingTermsAccepted" required class="mt-1 w-5 h-5 text-teal-600 border-2 border-gray-300 rounded focus:ring-teal-500">
                        <span class="text-gray-700">I confirm the booking details are correct and agree to the payment terms. ✅</span>
                    </label>
                </div>
                
                <div class="flex gap-4">
                    <button onclick="prevSlide()" class="flex-1 py-3 px-6 border-2 rounded-xl font-semibold transition-colors" style="border-color: #e88776; color: #e88776;">
                        ← Back
                    </button>
                    <button onclick="submitBooking()" class="flex-1 py-4 px-6 rounded-xl text-white font-semibold text-lg shadow-lg hover:shadow-xl transition-all" style="background-color: #18b79f;">
                        Confirm Booking! 🎉
                    </button>
                </div>
            </div>

            <!-- Slide 6: Success -->
            <div id="slide6" class="slide bg-white rounded-3xl shadow-lg p-8 text-center hidden">
                <div class="mb-6">
                    <div class="text-6xl mb-4">🎉</div>
                    <h1 class="text-3xl font-bold mb-4" style="color: #18b79f;">Booking Confirmed!</h1>
                    <p class="text-gray-600 text-lg mb-6">Your babysitting session has been successfully booked! 🧸</p>
                    
                    <div class="bg-gradient-to-r from-teal-50 to-pink-50 rounded-2xl p-6 mb-6">
                        <h3 class="text-xl font-semibold mb-3" style="color: #e88776;">Your Booking Reference:</h3>
                        <div class="text-3xl font-bold mb-3" style="color: #18b79f;" id="bookingReference">BK-00000</div>
                        <p class="text-sm text-gray-600">📸 Please screenshot this reference number!</p>
                    </div>
                    
                    <div class="bg-blue-50 rounded-2xl p-4 mb-6">
                        <p class="text-blue-700">📧 Confirmation emails have been sent to you and our team!</p>
                    </div>
                    
                    <div class="bg-gradient-to-r from-green-50 to-blue-50 rounded-2xl p-6 mb-6">
                        <h3 class="text-xl font-semibold mb-4" style="color: #18b79f;">What Happens Next? 🚀</h3>
                        <div class="space-y-3 text-left">
                            <div class="flex items-start gap-3">
                                <span class="text-2xl">1️⃣</span>
                                <div>
                                    <p class="font-semibold text-gray-800">We'll Assign Your Babysitter</p>
                                    <p class="text-gray-600 text-sm">You'll receive babysitter details 24 hours before your session</p>
                                </div>
                            </div>
                            <div class="flex items-start gap-3">
                                <span class="text-2xl">2️⃣</span>
                                <div>
                                    <p class="font-semibold text-gray-800">Pre-Session Contact</p>
                                    <p class="text-gray-600 text-sm">Your babysitter will contact you to confirm details</p>
                                </div>
                            </div>
                            <div class="flex items-start gap-3">
                                <span class="text-2xl">3️⃣</span>
                                <div>
                                    <p class="font-semibold text-gray-800">Enjoy Your Time Off!</p>
                                    <p class="text-gray-600 text-sm">Relax while your children have fun with their babysitter</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="space-y-4">
                    <button onclick="location.reload()" class="w-full py-4 px-6 rounded-2xl text-white font-semibold text-lg shadow-lg hover:shadow-xl transition-all" style="background-color: #e88776;">
                        Book Another Session 📅
                    </button>
                    <button onclick="window.open('https://gracefulgiggles.com', '_blank')" class="w-full py-3 px-6 border-2 rounded-xl font-semibold transition-colors" style="border-color: #18b79f; color: #18b79f;">
                        Visit Our Website 🌐
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Valid enrollment numbers database (simulated)
        const validEnrollmentNumbers = new Set([
            'GG-12345', 'GG-67890', 'GG-11111', 'GG-22222', 'GG-33333',
            'GG-44444', 'GG-55555', 'GG-66666', 'GG-77777', 'GG-88888'
        ]);
        
        // Family data (simulated database)
        const familyDatabase = {
            'GG-12345': {
                parentName: 'Sarah Johnson',
                parentEmail: 'sarah.johnson@email.com',
                parentPhone: '+356 99123456',
                address: '123 Main Street, Sliema SLM 1234',
                children: [
                    { name: 'Emma Johnson', age: '5 years old', allergies: 'None' },
                    { name: 'Liam Johnson', age: '3 years old', allergies: 'Nuts' }
                ],
                emergencyContact: 'Maria Johnson (+356 99654321)'
            },
            'GG-67890': {
                parentName: 'Michael Smith',
                parentEmail: 'michael.smith@email.com',
                parentPhone: '+356 99987654',
                address: '456 Oak Avenue, Valletta VLT 5678',
                children: [
                    { name: 'Sophia Smith', age: '7 years old', allergies: 'None' }
                ],
                emergencyContact: 'Anna Smith (+356 99111222)'
            }
        };
        
        let currentSlideIndex = 1;
        let currentFamily = null;
        let selectedDate = null;
        let selectedTime = null;
        let selectedDuration = null;
        let currentMonth = new Date().getMonth();
        let currentYear = new Date().getFullYear();
        let bookingMode = 'single';
        let selectedDates = [];
        let selectedSessions = [];
        let multiDayStartDate = null;
        let multiDayDays = null;
        let selectedChildren = [];
        let multiDaySelectedChildren = [];
        let multiDayCheckIn = null;
        let multiDayCheckOut = null;
        let isSelectingCheckOut = false;
        
        // Visual message system
        function showMessage(message, type = 'info') {
            const existingMsg = document.querySelector('.message-overlay');
            if (existingMsg) existingMsg.remove();
            
            const overlay = document.createElement('div');
            overlay.className = 'message-overlay fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50';
            
            const colors = {
                success: 'bg-green-100 border-green-500 text-green-800',
                error: 'bg-red-100 border-red-500 text-red-800',
                info: 'bg-blue-100 border-blue-500 text-blue-800'
            };
            
            overlay.innerHTML = `
                <div class="max-w-md mx-4 p-6 ${colors[type]} border-2 rounded-2xl shadow-lg">
                    <div class="whitespace-pre-line text-sm">${message}</div>
                    <button onclick="this.closest('.message-overlay').remove()" 
                            class="mt-4 px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition-colors">
                        Close
                    </button>
                </div>
            `;
            
            document.body.appendChild(overlay);
            
            if (type === 'success') {
                setTimeout(() => {
                    if (overlay.parentNode) overlay.remove();
                }, 5000);
            }
        }
        
        function verifyEnrollment() {
            const enrollmentInput = document.getElementById('enrollmentNumber');
            const inputValue = enrollmentInput.value.trim();
            
            if (!inputValue) {
                showMessage('Please enter your enrollment number', 'error');
                enrollmentInput.focus();
                return;
            }
            
            if (!/^\d{5}$/.test(inputValue)) {
                showMessage('Please enter exactly 5 digits (e.g., 12345)', 'error');
                enrollmentInput.focus();
                return;
            }
            
            const enrollmentNumber = 'GG-' + inputValue;
            
            if (!validEnrollmentNumbers.has(enrollmentNumber)) {
                showMessage('Invalid enrollment number. Please check your number or complete enrollment first.', 'error');
                enrollmentInput.focus();
                return;
            }
            
            const familyData = familyDatabase[enrollmentNumber];
            if (!familyData) {
                showMessage('Enrollment number found but family data not available. Please contact support.', 'error');
                return;
            }
            
            currentFamily = familyData;
            currentFamily.enrollmentNumber = enrollmentNumber;
            
            showMessage('Enrollment verified successfully! ✅', 'success');
            
            setTimeout(() => {
                populateFamilyInfo();
                nextSlide();
            }, 1500);
        }
        
        function populateFamilyInfo() {
            const familyInfoDiv = document.getElementById('familyInfo');
            
            const childrenList = currentFamily.children.map(child => 
                `<div class="flex justify-between items-center py-2 border-b border-gray-200 last:border-b-0">
                    <span class="font-medium">${child.name}</span>
                    <span class="text-sm text-gray-600">${child.age}</span>
                </div>`
            ).join('');
            
            familyInfoDiv.innerHTML = `
                <div class="flex items-center gap-3 mb-4">
                    <div class="text-3xl">👨‍👩‍👧‍👦</div>
                    <div>
                        <h3 class="text-xl font-semibold" style="color: #18b79f;">Family Verified!</h3>
                        <p class="text-gray-600">Enrollment: ${currentFamily.enrollmentNumber}</p>
                    </div>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div>
                        <h4 class="font-semibold text-gray-800 mb-2">Parent Details</h4>
                        <div class="space-y-1 text-sm text-gray-600">
                            <p><strong>Name:</strong> ${currentFamily.parentName}</p>
                            <p><strong>Email:</strong> ${currentFamily.parentEmail}</p>
                            <p><strong>Phone:</strong> ${currentFamily.parentPhone}</p>
                        </div>
                    </div>
                    
                    <div>
                        <h4 class="font-semibold text-gray-800 mb-2">Children (${currentFamily.children.length})</h4>
                        <div class="space-y-1">
                            ${childrenList}
                        </div>
                    </div>
                </div>
                
                <div class="mt-4 pt-4 border-t border-gray-200">
                    <p class="text-sm text-gray-600"><strong>Address:</strong> ${currentFamily.address}</p>
                    <p class="text-sm text-gray-600"><strong>Emergency Contact:</strong> ${currentFamily.emergencyContact}</p>
                </div>
            `;
            
            // Populate child selection lists
            populateChildSelectionLists();
        }
        
        function populateChildSelectionLists() {
            if (currentFamily.children.length > 1) {
                // Single date child selection
                const singleChildrenList = document.getElementById('singleChildrenList');
                singleChildrenList.innerHTML = currentFamily.children.map((child, index) => `
                    <label class="flex items-center gap-3 p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors cursor-pointer">
                        <input type="checkbox" value="${index}" onchange="updateSingleChildSelection(${index}, this.checked)" class="text-teal-600 w-5 h-5">
                        <div class="flex-1">
                            <div class="font-medium text-gray-800">${child.name}</div>
                            <div class="text-sm text-gray-600">${child.age}</div>
                            ${child.allergies !== 'None' ? `<div class="text-xs text-red-600">⚠️ Allergies: ${child.allergies}</div>` : ''}
                        </div>
                    </label>
                `).join('');
                
                // Multi-day child selection
                const multiDayChildrenList = document.getElementById('multiDayChildrenList');
                multiDayChildrenList.innerHTML = currentFamily.children.map((child, index) => `
                    <label class="flex items-center gap-3 p-4 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-colors cursor-pointer">
                        <input type="checkbox" value="${index}" onchange="updateMultiDayChildSelection(${index}, this.checked)" class="text-teal-600 w-5 h-5">
                        <div class="flex-1">
                            <div class="font-medium text-gray-800">${child.name}</div>
                            <div class="text-sm text-gray-600">${child.age}</div>
                            ${child.allergies !== 'None' ? `<div class="text-xs text-red-600">⚠️ Allergies: ${child.allergies}</div>` : ''}
                        </div>
                    </label>
                `).join('');
            }
        }
        
        function updateSingleChildSelection(childIndex, isChecked) {
            if (isChecked) {
                if (!selectedChildren.includes(childIndex)) {
                    selectedChildren.push(childIndex);
                }
            } else {
                const index = selectedChildren.indexOf(childIndex);
                if (index > -1) {
                    selectedChildren.splice(index, 1);
                }
            }
        }
        
        function updateMultiDayChildSelection(childIndex, isChecked) {
            if (isChecked) {
                if (!multiDaySelectedChildren.includes(childIndex)) {
                    multiDaySelectedChildren.push(childIndex);
                }
            } else {
                const index = multiDaySelectedChildren.indexOf(childIndex);
                if (index > -1) {
                    multiDaySelectedChildren.splice(index, 1);
                }
            }
        }
        
        function initializeCalendar() {
            updateCalendarHeader();
            generateCalendar();
            generateTimeSlots();
        }
        
        function updateCalendarHeader() {
            const monthNames = ['January', 'February', 'March', 'April', 'May', 'June',
                'July', 'August', 'September', 'October', 'November', 'December'];
            document.getElementById('currentMonth').textContent = `${monthNames[currentMonth]} ${currentYear}`;
        }
        
        function changeMonth(direction) {
            currentMonth += direction;
            if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            } else if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            }
            
            // Don't allow going to past months
            const today = new Date();
            if (currentYear < today.getFullYear() || 
                (currentYear === today.getFullYear() && currentMonth < today.getMonth())) {
                currentMonth = today.getMonth();
                currentYear = today.getFullYear();
            }
            
            updateCalendarHeader();
            generateCalendar();
        }
        
        function generateCalendar() {
            const calendarGrid = document.getElementById('calendarGrid');
            const firstDay = new Date(currentYear, currentMonth, 1).getDay();
            const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
            const today = new Date();
            
            calendarGrid.innerHTML = '';
            
            // Empty cells for days before the first day of the month
            for (let i = 0; i < firstDay; i++) {
                const emptyCell = document.createElement('div');
                calendarGrid.appendChild(emptyCell);
            }
            
            // Days of the month
            for (let day = 1; day <= daysInMonth; day++) {
                const dayElement = document.createElement('div');
                const dayDate = new Date(currentYear, currentMonth, day);
                const isToday = dayDate.toDateString() === today.toDateString();
                const isPast = dayDate < today && !isToday;
                
                dayElement.className = `calendar-day text-center py-3 rounded-lg ${isPast ? 'disabled' : ''}`;
                dayElement.textContent = day;
                
                if (isToday) {
                    dayElement.classList.add('bg-blue-100', 'font-semibold');
                }
                
                if (!isPast) {
                    dayElement.onclick = () => selectDate(currentYear, currentMonth, day);
                }
                
                calendarGrid.appendChild(dayElement);
            }
            
            // Re-apply multi-day highlights if they exist
            if (bookingMode === 'multiday' && multiDayCheckIn) {
                highlightDateRange();
            }
        }
        
        function selectDate(year, month, day) {
            const clickedDate = new Date(year, month, day);
            const dateString = clickedDate.toDateString();
            
            if (bookingMode === 'multiple') {
                // Multiple dates mode
                const existingIndex = selectedDates.findIndex(date => date.toDateString() === dateString);
                
                if (existingIndex >= 0) {
                    // Remove date if already selected
                    selectedDates.splice(existingIndex, 1);
                    event.target.classList.remove('selected');
                } else {
                    // Add date to selection
                    selectedDates.push(clickedDate);
                    event.target.classList.add('selected');
                }
                
                updateSelectedDatesDisplay();
            } else if (bookingMode === 'multiday') {
                // Multi-day booking.com style selection
                if (!multiDayCheckIn || (multiDayCheckIn && multiDayCheckOut)) {
                    // First click or reset - select check-in date
                    clearMultiDayCalendarHighlights();
                    multiDayCheckIn = clickedDate;
                    multiDayCheckOut = null;
                    isSelectingCheckOut = true;
                    
                    event.target.classList.add('check-in');
                    updateMultiDayDisplays();
                } else if (isSelectingCheckOut) {
                    // Second click - select check-out date
                    if (clickedDate <= multiDayCheckIn) {
                        // If clicked date is before or same as check-in, reset and make it new check-in
                        clearMultiDayCalendarHighlights();
                        multiDayCheckIn = clickedDate;
                        multiDayCheckOut = null;
                        event.target.classList.add('check-in');
                    } else {
                        // Valid check-out date
                        multiDayCheckOut = clickedDate;
                        isSelectingCheckOut = false;
                        event.target.classList.add('check-out');
                        highlightDateRange();
                    }
                    updateMultiDayDisplays();
                }
            } else if (bookingMode === 'single') {
                // Single date mode
                document.querySelectorAll('.calendar-day.selected').forEach(el => {
                    el.classList.remove('selected');
                });
                
                event.target.classList.add('selected');
                selectedDate = clickedDate;
            }
        }
        
        function clearMultiDayCalendarHighlights() {
            document.querySelectorAll('.calendar-day').forEach(el => {
                el.classList.remove('check-in', 'check-out', 'in-range');
            });
        }
        
        function highlightDateRange() {
            if (!multiDayCheckIn || !multiDayCheckOut) return;
            
            const startDate = new Date(multiDayCheckIn);
            const endDate = new Date(multiDayCheckOut);
            
            // Clear existing highlights
            clearMultiDayCalendarHighlights();
            
            // Highlight all dates in the current calendar view
            document.querySelectorAll('.calendar-day').forEach(dayEl => {
                const dayText = dayEl.textContent.trim();
                if (!dayText || isNaN(dayText)) return;
                
                const dayDate = new Date(currentYear, currentMonth, parseInt(dayText));
                
                if (dayDate.toDateString() === startDate.toDateString()) {
                    dayEl.classList.add('check-in');
                } else if (dayDate.toDateString() === endDate.toDateString()) {
                    dayEl.classList.add('check-out');
                } else if (dayDate > startDate && dayDate < endDate) {
                    dayEl.classList.add('in-range');
                }
            });
        }
        
        function updateMultiDayDisplays() {
            const checkInDisplay = document.getElementById('checkInDisplay');
            const checkOutDisplay = document.getElementById('checkOutDisplay');
            const previewDiv = document.getElementById('multiDayPreview');
            const datesDiv = document.getElementById('multiDayDates');
            const clearBtn = document.getElementById('clearMultiDayDates');
            
            // Update check-in display
            if (multiDayCheckIn) {
                checkInDisplay.textContent = multiDayCheckIn.toLocaleDateString('en-GB', { 
                    weekday: 'short', 
                    month: 'short', 
                    day: 'numeric' 
                });
                checkInDisplay.classList.remove('text-green-700');
                checkInDisplay.classList.add('text-green-700');
                clearBtn.classList.remove('hidden');
            } else {
                checkInDisplay.textContent = 'Not selected';
                checkInDisplay.classList.remove('text-green-700');
                clearBtn.classList.add('hidden');
            }
            
            // Update check-out display
            if (multiDayCheckOut) {
                checkOutDisplay.textContent = multiDayCheckOut.toLocaleDateString('en-GB', { 
                    weekday: 'short', 
                    month: 'short', 
                    day: 'numeric' 
                });
                checkOutDisplay.classList.remove('text-blue-700');
                checkOutDisplay.classList.add('text-blue-700');
            } else {
                checkOutDisplay.textContent = isSelectingCheckOut ? 'Select check-out...' : 'Not selected';
                checkOutDisplay.classList.remove('text-blue-700');
            }
            
            // Update preview
            if (multiDayCheckIn && multiDayCheckOut) {
                const timeDiff = multiDayCheckOut.getTime() - multiDayCheckIn.getTime();
                const daysDiff = Math.ceil(timeDiff / (1000 * 3600 * 24));
                const nights = daysDiff - 1;
                
                // Update global variables for validation
                multiDayStartDate = multiDayCheckIn;
                multiDayDays = daysDiff;
                
                datesDiv.innerHTML = `
                    <div class="space-y-2">
                        <div><strong>Start Date:</strong> ${multiDayCheckIn.toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
                        <div><strong>End Date:</strong> ${multiDayCheckOut.toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
                        <div><strong>Duration:</strong> ${daysDiff} days, ${nights} night${nights !== 1 ? 's' : ''}</div>
                        <div class="text-teal-600 font-medium">🌙 Includes overnight care and 24/7 supervision</div>
                    </div>
                `;
                
                previewDiv.classList.remove('hidden');
            } else {
                previewDiv.classList.add('hidden');
                multiDayStartDate = null;
                multiDayDays = null;
            }
        }
        
        function clearMultiDaySelection() {
            multiDayCheckIn = null;
            multiDayCheckOut = null;
            isSelectingCheckOut = false;
            multiDayStartDate = null;
            multiDayDays = null;
            
            clearMultiDayCalendarHighlights();
            updateMultiDayDisplays();
        }
        
        function changeBookingMode(mode) {
            bookingMode = mode;
            const singleTimeSelection = document.getElementById('singleDateTimeSelection');
            const selectedDatesList = document.getElementById('selectedDatesList');
            const multiDayConfig = document.getElementById('multiDayConfig');
            const dateTitle = document.getElementById('dateSelectionTitle');
            const clearBtn = document.getElementById('clearDatesBtn');
            const singleChildSelection = document.getElementById('singleChildSelection');
            const multiDayChildSelection = document.getElementById('multiDayChildSelection');
            
            // Hide all mode-specific sections
            singleTimeSelection.classList.add('hidden');
            selectedDatesList.classList.add('hidden');
            multiDayConfig.classList.add('hidden');
            clearBtn.classList.add('hidden');
            singleChildSelection.classList.add('hidden');
            multiDayChildSelection.classList.add('hidden');
            
            // Clear all selections
            selectedDate = null;
            selectedTime = null;
            selectedDuration = null;
            selectedDates = [];
            selectedSessions = [];
            multiDayStartDate = null;
            multiDayDays = null;
            selectedChildren = [];
            multiDaySelectedChildren = [];
            
            // Clear multi-day specific selections
            multiDayCheckIn = null;
            multiDayCheckOut = null;
            isSelectingCheckOut = false;
            
            document.querySelectorAll('.calendar-day.selected').forEach(el => {
                el.classList.remove('selected');
            });
            clearMultiDayCalendarHighlights();
            document.querySelectorAll('.time-slot.selected').forEach(el => {
                el.classList.remove('selected');
            });
            document.querySelectorAll('.duration-btn').forEach(btn => {
                btn.classList.remove('border-teal-400', 'bg-teal-50');
                btn.classList.add('border-gray-200');
            });
            
            // Clear child selection checkboxes
            document.querySelectorAll('#singleChildrenList input[type="checkbox"]').forEach(cb => cb.checked = false);
            document.querySelectorAll('#multiDayChildrenList input[type="checkbox"]').forEach(cb => cb.checked = false);
            
            // Show appropriate sections based on mode
            if (mode === 'single') {
                dateTitle.textContent = 'Choose Your Date';
                singleTimeSelection.classList.remove('hidden');
                if (currentFamily && currentFamily.children.length > 1) {
                    singleChildSelection.classList.remove('hidden');
                }
            } else if (mode === 'multiple') {
                dateTitle.textContent = 'Choose Your Dates';
                selectedDatesList.classList.remove('hidden');
                clearBtn.classList.remove('hidden');
            } else if (mode === 'multiday') {
                dateTitle.textContent = 'Select Start & End Dates';
                multiDayConfig.classList.remove('hidden');
                if (currentFamily && currentFamily.children.length > 1) {
                    multiDayChildSelection.classList.remove('hidden');
                }
            }
            
            updateSelectedDatesDisplay();
        }
        

        
        function clearSelectedDates() {
            selectedDates = [];
            selectedSessions = [];
            document.querySelectorAll('.calendar-day.selected').forEach(el => {
                el.classList.remove('selected');
            });
            updateSelectedDatesDisplay();
        }
        
        function updateSelectedDatesDisplay() {
            const countSpan = document.getElementById('selectedDatesCount');
            const container = document.getElementById('selectedDatesContainer');
            
            if (selectedDates.length === 0) {
                countSpan.textContent = '';
                container.innerHTML = '<p class="text-gray-500 text-center py-4">No dates selected yet. Click on calendar dates to add sessions.</p>';
                return;
            }
            
            countSpan.textContent = `(${selectedDates.length} selected)`;
            
            // Sort dates chronologically
            const sortedDates = [...selectedDates].sort((a, b) => a - b);
            
            container.innerHTML = sortedDates.map((date, index) => {
                const dateString = date.toLocaleDateString('en-GB', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });
                
                const session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
                
                return `
                    <div class="bg-gradient-to-r from-teal-50 to-blue-50 rounded-xl p-4 border-2 border-teal-200">
                        <div class="flex justify-between items-start">
                            <div class="flex-1">
                                <h4 class="font-semibold text-gray-800">${dateString}</h4>
                                <div class="mt-2 space-y-2">
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Start Time</label>
                                        <select onchange="updateSessionTime(${index}, this.value)" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                                            <option value="">Select time...</option>
                                            ${['05:00', '06:00', '07:00', '08:00', '09:00', '10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00', '17:00', '18:00', '19:00', '20:00', '21:00', '22:00', '23:00', '00:00'].map(time => 
                                                `<option value="${time}" ${session && session.time === time ? 'selected' : ''}>${time}</option>`
                                            ).join('')}
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">Duration *</label>
                                        <div class="grid grid-cols-1 gap-2">
                                            <button onclick="updateSessionDuration(${index}, 1)" class="session-duration-btn p-2 border-2 border-gray-200 rounded-lg hover:border-teal-400 transition-colors text-sm ${session && session.duration === 1 ? 'border-teal-400 bg-teal-50' : ''}">
                                                <div class="font-semibold">1 Hour</div>
                                            </button>
                                            <button onclick="updateSessionDuration(${index}, 2)" class="session-duration-btn p-2 border-2 border-gray-200 rounded-lg hover:border-teal-400 transition-colors text-sm ${session && session.duration === 2 ? 'border-teal-400 bg-teal-50' : ''}">
                                                <div class="font-semibold">2 Hours</div>
                                            </button>
                                            <button onclick="updateSessionDuration(${index}, 3)" class="session-duration-btn p-2 border-2 border-gray-200 rounded-lg hover:border-teal-400 transition-colors text-sm ${session && session.duration === 3 ? 'border-teal-400 bg-teal-50' : ''}">
                                                <div class="font-semibold">3 Hours</div>
                                            </button>
                                            <button onclick="updateSessionDuration(${index}, 4)" class="session-duration-btn p-2 border-2 border-gray-200 rounded-lg hover:border-teal-400 transition-colors text-sm ${session && session.duration === 4 ? 'border-teal-400 bg-teal-50' : ''}">
                                                <div class="font-semibold">4 Hours</div>
                                            </button>
                                            <button onclick="updateSessionDuration(${index}, 'flexible')" class="session-duration-btn p-2 border-2 border-gray-200 rounded-lg hover:border-teal-400 transition-colors text-sm ${session && session.duration === 'flexible' ? 'border-teal-400 bg-teal-50' : ''}">
                                                <div class="font-semibold">Don't Know</div>
                                                <div class="text-xs text-gray-600">Flexible</div>
                                            </button>
                                        </div>
                                    </div>
                                    
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-2">Which Children? *</label>
                                        <div class="space-y-2">
                                            ${currentFamily.children.map((child, childIndex) => `
                                                <label class="flex items-center gap-2 p-2 border border-gray-200 rounded-lg hover:border-teal-400 transition-colors cursor-pointer text-sm">
                                                    <input type="checkbox" onchange="updateSessionChildren(${index}, ${childIndex}, this.checked)" class="text-teal-600" ${session && session.children && session.children.includes(childIndex) ? 'checked' : ''}>
                                                    <div class="flex-1">
                                                        <div class="font-medium">${child.name}</div>
                                                        <div class="text-xs text-gray-500">${child.age}</div>
                                                    </div>
                                                </label>
                                            `).join('')}
                                        </div>
                                    </div>

                                </div>
                            </div>
                            <button onclick="removeSelectedDate(${index})" class="ml-4 p-2 text-red-600 hover:bg-red-100 rounded-lg transition-colors">
                                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                                </svg>
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
        }
        
        function updateSessionTime(index, time) {
            const date = selectedDates[index];
            let session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
            
            if (!session) {
                session = { date: date, time: null, duration: null };
                selectedSessions.push(session);
            }
            
            session.time = time;
        }
        
        function updateSessionDuration(index, duration) {
            const date = selectedDates[index];
            let session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
            
            if (!session) {
                session = { date: date, time: null, duration: null };
                selectedSessions.push(session);
            }
            
            if (duration === 'flexible') {
                session.duration = 'flexible';
            } else {
                session.duration = parseInt(duration);
            }
            
            // Refresh display to update button states
            updateSelectedDatesDisplay();
        }
        
        function updateSessionChildren(sessionIndex, childIndex, isChecked) {
            const date = selectedDates[sessionIndex];
            let session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
            
            if (!session) {
                session = { date: date, time: null, duration: null, children: [] };
                selectedSessions.push(session);
            }
            
            if (!session.children) {
                session.children = [];
            }
            
            if (isChecked) {
                if (!session.children.includes(childIndex)) {
                    session.children.push(childIndex);
                }
            } else {
                const index = session.children.indexOf(childIndex);
                if (index > -1) {
                    session.children.splice(index, 1);
                }
            }
        }
        
        function removeSelectedDate(index) {
            const dateToRemove = selectedDates[index];
            
            // Remove from selectedDates array
            selectedDates.splice(index, 1);
            
            // Remove from selectedSessions array
            const sessionIndex = selectedSessions.findIndex(s => s.date.toDateString() === dateToRemove.toDateString());
            if (sessionIndex >= 0) {
                selectedSessions.splice(sessionIndex, 1);
            }
            
            // Remove visual selection from calendar
            document.querySelectorAll('.calendar-day').forEach(el => {
                const dayText = el.textContent;
                const dayDate = new Date(currentYear, currentMonth, parseInt(dayText));
                if (dayDate.toDateString() === dateToRemove.toDateString()) {
                    el.classList.remove('selected');
                }
            });
            
            updateSelectedDatesDisplay();
        }
        
        function generateTimeSlots() {
            const timeSlotsContainer = document.getElementById('timeSlots');
            const timeSlots = [
                '05:00', '06:00', '07:00', '08:00', '09:00', '10:00',
                '11:00', '12:00', '13:00', '14:00', '15:00', '16:00',
                '17:00', '18:00', '19:00', '20:00', '21:00', '22:00',
                '23:00', '00:00'
            ];
            
            timeSlotsContainer.innerHTML = '';
            
            timeSlots.forEach(time => {
                const timeSlot = document.createElement('button');
                timeSlot.className = 'time-slot p-3 border-2 border-gray-200 rounded-xl hover:border-teal-400 transition-all font-medium';
                timeSlot.textContent = time;
                timeSlot.onclick = () => selectTime(time, timeSlot);
                timeSlotsContainer.appendChild(timeSlot);
            });
        }
        
        function selectTime(time, element) {
            // Remove previous selection
            document.querySelectorAll('.time-slot.selected').forEach(el => {
                el.classList.remove('selected');
            });
            
            // Add selection to clicked time
            element.classList.add('selected');
            selectedTime = time;
        }
        
        function selectDuration(hours) {
            // Remove previous selection
            document.querySelectorAll('.duration-btn').forEach(btn => {
                btn.classList.remove('border-teal-400', 'bg-teal-50');
                btn.classList.add('border-gray-200');
            });
            
            // Add selection to clicked duration
            event.target.closest('.duration-btn').classList.remove('border-gray-200');
            event.target.closest('.duration-btn').classList.add('border-teal-400', 'bg-teal-50');
            
            if (hours === 'flexible') {
                selectedDuration = 'flexible';
            } else {
                selectedDuration = parseInt(hours);
            }
            
            // Update transport options when duration changes
            populateTransportOptions();
        }
        
        function toggleAlternateContact() {
            const checkbox = document.getElementById('useAlternateContact');
            const detailsDiv = document.getElementById('alternateContactDetails');
            
            if (checkbox.checked) {
                detailsDiv.classList.remove('hidden');
            } else {
                detailsDiv.classList.add('hidden');
            }
        }
        
        function togglePickupDetails() {
            const checkbox = document.getElementById('needPickup');
            const detailsDiv = document.getElementById('pickupDetails');
            const pickupLocation = document.getElementById('pickupLocation');
            const pickupTime = document.getElementById('pickupTime');
            
            if (checkbox.checked) {
                detailsDiv.classList.remove('hidden');
                pickupLocation.required = true;
                pickupTime.required = true;
            } else {
                detailsDiv.classList.add('hidden');
                pickupLocation.required = false;
                pickupTime.required = false;
                // Clear values
                pickupLocation.value = '';
                pickupTime.value = '';
                document.getElementById('pickupInstructions').value = '';
            }
        }
        
        function toggleDropoffDetails() {
            const checkbox = document.getElementById('needDropoff');
            const detailsDiv = document.getElementById('dropoffDetails');
            const dropoffLocation = document.getElementById('dropoffLocation');
            const dropoffTime = document.getElementById('dropoffTime');
            
            if (checkbox.checked) {
                detailsDiv.classList.remove('hidden');
                dropoffLocation.required = true;
                dropoffTime.required = true;
            } else {
                detailsDiv.classList.add('hidden');
                dropoffLocation.required = false;
                dropoffTime.required = false;
                // Clear values
                dropoffLocation.value = '';
                dropoffTime.value = '';
                document.getElementById('dropoffInstructions').value = '';
            }
        }
        
        function setupLocationRadios() {
            const locationRadios = document.querySelectorAll('input[name="location"]');
            const publicLocationDetails = document.getElementById('publicLocationDetails');
            
            locationRadios.forEach(radio => {
                radio.addEventListener('change', function() {
                    if (this.value === 'public') {
                        publicLocationDetails.classList.remove('hidden');
                        document.getElementById('publicLocationInput').required = true;
                    } else {
                        publicLocationDetails.classList.add('hidden');
                        document.getElementById('publicLocationInput').required = false;
                    }
                });
            });
        }
        
        function isTransportEligible(duration) {
            if (duration === 'flexible') return true;
            return duration >= 3;
        }
        
        function populateTransportOptions() {
            const transportContainer = document.getElementById('transportOptions');
            
            // Check eligibility for single date mode
            if (bookingMode === 'single') {
                if (!selectedDuration || !isTransportEligible(selectedDuration)) {
                    transportContainer.innerHTML = `
                        <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-lg">
                            <div class="flex items-start">
                                <div class="text-yellow-400 text-lg mr-2">ℹ️</div>
                                <div class="text-sm text-yellow-700">
                                    <strong>Transport services are only available for sessions of 3+ hours.</strong><br>
                                    Please select a longer duration to access pickup/drop-off options.
                                </div>
                            </div>
                        </div>
                    `;
                    return;
                }
            }
            
            if (bookingMode === 'multiple' && selectedDates.length > 0) {
                // Multiple dates mode - show options for each date
                const sortedDates = [...selectedDates].sort((a, b) => a - b);
                
                transportContainer.innerHTML = `
                    <div class="space-y-4">
                        <h4 class="font-medium text-gray-800">Select dates that need transport:</h4>
                        ${sortedDates.map((date, index) => {
                            const dateString = date.toLocaleDateString('en-GB', { 
                                weekday: 'short', 
                                month: 'short', 
                                day: 'numeric' 
                            });
                            
                            const session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
                            const isEligible = session && isTransportEligible(session.duration);
                            
                            if (!isEligible) {
                                return `
                                    <div class="bg-gray-50 rounded-lg p-4 opacity-60">
                                        <h5 class="font-medium text-gray-800 mb-2">${dateString}</h5>
                                        <p class="text-sm text-gray-600">Transport not available - session must be 3+ hours</p>
                                    </div>
                                `;
                            }
                            
                            return `
                                <div class="bg-gray-50 rounded-lg p-4">
                                    <h5 class="font-medium text-gray-800 mb-3">${dateString}</h5>
                                    <div class="space-y-3">
                                        <label class="flex items-center gap-3 cursor-pointer">
                                            <input type="checkbox" id="pickup_${index}" class="text-teal-600" onchange="toggleDateTransport(${index}, 'pickup', this.checked)">
                                            <span class="font-medium">📍 Pickup Required</span>
                                        </label>
                                        
                                        <div id="pickupDetails_${index}" class="hidden ml-6 space-y-2 bg-white rounded-lg p-3">
                                            <input type="text" id="pickupLocation_${index}" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Pickup location">
                                            <input type="time" id="pickupTime_${index}" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                                            <textarea id="pickupInstructions_${index}" rows="2" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Pickup instructions (optional)"></textarea>
                                        </div>
                                        
                                        <label class="flex items-center gap-3 cursor-pointer">
                                            <input type="checkbox" id="dropoff_${index}" class="text-teal-600" onchange="toggleDateTransport(${index}, 'dropoff', this.checked)">
                                            <span class="font-medium">🎯 Drop-off Required</span>
                                        </label>
                                        
                                        <div id="dropoffDetails_${index}" class="hidden ml-6 space-y-2 bg-white rounded-lg p-3">
                                            <input type="text" id="dropoffLocation_${index}" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Drop-off location">
                                            <input type="time" id="dropoffTime_${index}" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                                            <textarea id="dropoffInstructions_${index}" rows="2" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Drop-off instructions (optional)"></textarea>
                                        </div>
                                    </div>
                                </div>
                            `;
                        }).join('')}
                    </div>
                `;
            } else if (bookingMode === 'multiday') {
                // Multi-day mode - transport is included in the service
                transportContainer.innerHTML = `
                    <div class="bg-gradient-to-r from-purple-50 to-pink-50 rounded-2xl p-6">
                        <div class="flex items-start gap-3 mb-4">
                            <div class="text-2xl">🚗</div>
                            <div>
                                <h4 class="font-semibold text-gray-800 mb-2">Transport Included</h4>
                                <p class="text-sm text-gray-700 mb-4">Multi-day stays include transport services as needed during the stay period.</p>
                            </div>
                        </div>
                        
                        <div class="space-y-4">
                            <div>
                                <label class="flex items-center gap-3 cursor-pointer">
                                    <input type="checkbox" id="multiDayInitialPickup" class="text-teal-600">
                                    <span class="font-medium">📍 Initial Pickup Required</span>
                                </label>
                                <p class="text-xs text-gray-600 ml-6">Pick up child at start of sitting period</p>
                                
                                <div id="multiDayPickupDetails" class="hidden ml-6 mt-3 space-y-2 bg-white rounded-lg p-3">
                                    <input type="text" id="multiDayPickupLocation" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Pickup location">
                                    <input type="time" id="multiDayPickupTime" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                                    <textarea id="multiDayPickupInstructions" rows="2" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Pickup instructions (optional)"></textarea>
                                </div>
                            </div>
                            
                            <div>
                                <label class="flex items-center gap-3 cursor-pointer">
                                    <input type="checkbox" id="multiDayFinalDropoff" class="text-teal-600">
                                    <span class="font-medium">🎯 Final Drop-off Required</span>
                                </label>
                                <p class="text-xs text-gray-600 ml-6">Drop off child at end of sitting period</p>
                                
                                <div id="multiDayDropoffDetails" class="hidden ml-6 mt-3 space-y-2 bg-white rounded-lg p-3">
                                    <input type="text" id="multiDayDropoffLocation" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Drop-off location">
                                    <input type="time" id="multiDayDropoffTime" class="w-full p-2 border border-gray-300 rounded-lg text-sm">
                                    <textarea id="multiDayDropoffInstructions" rows="2" class="w-full p-2 border border-gray-300 rounded-lg text-sm" placeholder="Drop-off instructions (optional)"></textarea>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mt-4 p-3 bg-blue-50 border-l-4 border-blue-400 rounded-r-lg">
                            <div class="flex items-start">
                                <div class="text-blue-400 text-lg mr-2">ℹ️</div>
                                <div class="text-sm text-blue-700">
                                    <strong>Additional transport during sitting:</strong><br>
                                    Any school runs, activities, or appointments during the sitting period will be handled by the babysitter as needed.
                                </div>
                            </div>
                        </div>
                    </div>
                `;
                
                // Add event listeners for multi-day transport
                document.getElementById('multiDayInitialPickup').addEventListener('change', function() {
                    const details = document.getElementById('multiDayPickupDetails');
                    if (this.checked) {
                        details.classList.remove('hidden');
                    } else {
                        details.classList.add('hidden');
                    }
                });
                
                document.getElementById('multiDayFinalDropoff').addEventListener('change', function() {
                    const details = document.getElementById('multiDayDropoffDetails');
                    if (this.checked) {
                        details.classList.remove('hidden');
                    } else {
                        details.classList.add('hidden');
                    }
                });
            } else {
                // Single date mode - simple pickup/dropoff options
                transportContainer.innerHTML = `
                    <div class="space-y-3">
                        <label class="flex items-center gap-3 cursor-pointer">
                            <input type="checkbox" id="needPickup" onchange="togglePickupDetails()" class="text-teal-600">
                            <span class="font-medium">📍 Pickup Required</span>
                        </label>
                        
                        <div id="pickupDetails" class="hidden ml-6 space-y-3 bg-gray-50 rounded-lg p-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Pickup Location *</label>
                                <input type="text" id="pickupLocation" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="e.g., St. Catherine's School, Pembroke">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Pickup Time *</label>
                                <input type="time" id="pickupTime" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Pickup Instructions</label>
                                <textarea id="pickupInstructions" rows="2" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="e.g., Wait at main gate, child will be wearing blue uniform..."></textarea>
                            </div>
                        </div>
                        
                        <label class="flex items-center gap-3 cursor-pointer">
                            <input type="checkbox" id="needDropoff" onchange="toggleDropoffDetails()" class="text-teal-600">
                            <span class="font-medium">🎯 Drop-off Required</span>
                        </label>
                        
                        <div id="dropoffDetails" class="hidden ml-6 space-y-3 bg-gray-50 rounded-lg p-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Drop-off Location *</label>
                                <input type="text" id="dropoffLocation" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="e.g., Grandma's house, Swimming lessons at Neptunes">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Drop-off Time *</label>
                                <input type="time" id="dropoffTime" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Drop-off Instructions</label>
                                <textarea id="dropoffInstructions" rows="2" class="w-full p-3 border-2 border-gray-200 rounded-xl focus:border-teal-400 focus:outline-none transition-colors" placeholder="e.g., Ring doorbell, hand over to Mrs. Smith..."></textarea>
                            </div>
                        </div>
                    </div>
                `;
            }
        }
        
        function toggleDateTransport(dateIndex, type, isChecked) {
            const detailsDiv = document.getElementById(`${type}Details_${dateIndex}`);
            
            if (isChecked) {
                detailsDiv.classList.remove('hidden');
            } else {
                detailsDiv.classList.add('hidden');
                // Clear the inputs
                document.getElementById(`${type}Location_${dateIndex}`).value = '';
                document.getElementById(`${type}Time_${dateIndex}`).value = '';
                document.getElementById(`${type}Instructions_${dateIndex}`).value = '';
            }
        }
        
        function updateProgress() {
            const progress = (currentSlideIndex / 5) * 100;
            document.getElementById('progressBar').style.width = progress + '%';
            document.getElementById('currentStep').textContent = currentSlideIndex;
        }
        
        function showSlide(index) {
            document.querySelectorAll('.slide').forEach(slide => {
                slide.classList.add('hidden');
            });
            
            const currentSlide = document.getElementById(`slide${index}`);
            if (currentSlide) {
                currentSlide.classList.remove('hidden');
                currentSlide.classList.add('slide-enter');
                
                if (index === 3) {
                    initializeCalendar();
                }
                
                if (index === 4) {
                    setupLocationRadios();
                    populateTransportOptions();
                }
                
                if (index === 5) {
                    generateBookingSummary();
                }
                
                updateProgress();
            }
        }
        
        function nextSlide() {
            if (!validateCurrentSlide()) {
                return;
            }
            
            if (currentSlideIndex < 5) {
                currentSlideIndex++;
                showSlide(currentSlideIndex);
            }
        }
        
        function prevSlide() {
            if (currentSlideIndex > 1) {
                currentSlideIndex--;
                showSlide(currentSlideIndex);
            }
        }
        
        function validateCurrentSlide() {
            if (currentSlideIndex === 3) {
                if (bookingMode === 'multiple') {
                    // Validate multiple dates mode
                    if (selectedDates.length === 0) {
                        showMessage('Please select at least one date', 'error');
                        return false;
                    }
                    
                    // Check that all selected dates have time, duration, and children
                    for (let date of selectedDates) {
                        const session = selectedSessions.find(s => s.date.toDateString() === date.toDateString());
                        if (!session || !session.time) {
                            showMessage(`Please select a start time for ${date.toLocaleDateString('en-GB')}`, 'error');
                            return false;
                        }
                        if (!session.duration) {
                            showMessage(`Please select a duration for ${date.toLocaleDateString('en-GB')}`, 'error');
                            return false;
                        }
                        if (!session.children || session.children.length === 0) {
                            showMessage(`Please select which children for ${date.toLocaleDateString('en-GB')}`, 'error');
                            return false;
                        }
                    }
                } else if (bookingMode === 'multiday') {
                    // Validate multi-day mode
                    if (!multiDayStartDate) {
                        showMessage('Please select a start date for your multi-day stay', 'error');
                        return false;
                    }
                    
                    if (!multiDayDays) {
                        showMessage('Please select the number of days for your stay', 'error');
                        return false;
                    }
                    
                    // Validate child selection for families with multiple children
                    if (currentFamily.children.length > 1 && multiDaySelectedChildren.length === 0) {
                        showMessage('Please select which children will be staying', 'error');
                        return false;
                    }
                } else {
                    // Validate single date mode
                    if (!selectedDate) {
                        showMessage('Please select a date', 'error');
                        return false;
                    }
                    
                    if (!selectedTime) {
                        showMessage('Please select a start time', 'error');
                        return false;
                    }
                    
                    if (selectedDuration === null || selectedDuration === undefined) {
                        showMessage('Please select a duration', 'error');
                        return false;
                    }
                    
                    // Validate child selection for families with multiple children
                    if (currentFamily.children.length > 1 && selectedChildren.length === 0) {
                        showMessage('Please select which children will attend this session', 'error');
                        return false;
                    }
                }
            }
            
            if (currentSlideIndex === 4) {
                const locationSelected = document.querySelector('input[name="location"]:checked');
                if (!locationSelected) {
                    showMessage('Please select a session location', 'error');
                    return false;
                }
                
                if (locationSelected.value === 'public') {
                    const publicLocation = document.getElementById('publicLocationInput').value.trim();
                    if (!publicLocation) {
                        showMessage('Please specify the public location', 'error');
                        return false;
                    }
                }
                
                const useAlternate = document.getElementById('useAlternateContact').checked;
                if (useAlternate) {
                    const altName = document.getElementById('alternateContactName').value.trim();
                    const altPhone = document.getElementById('alternateContactPhone').value.trim();
                    if (!altName || !altPhone) {
                        showMessage('Please provide alternate contact details', 'error');
                        return false;
                    }
                }
                
                // Validate pickup details (only in single date mode)
                if (bookingMode === 'single') {
                    const needPickup = document.getElementById('needPickup');
                    if (needPickup && needPickup.checked) {
                        const pickupLocation = document.getElementById('pickupLocation').value.trim();
                        const pickupTime = document.getElementById('pickupTime').value;
                        if (!pickupLocation || !pickupTime) {
                            showMessage('Please provide pickup location and time', 'error');
                            return false;
                        }
                    }
                    
                    // Validate drop-off details
                    const needDropoff = document.getElementById('needDropoff');
                    if (needDropoff && needDropoff.checked) {
                        const dropoffLocation = document.getElementById('dropoffLocation').value.trim();
                        const dropoffTime = document.getElementById('dropoffTime').value;
                        if (!dropoffLocation || !dropoffTime) {
                            showMessage('Please provide drop-off location and time', 'error');
                            return false;
                        }
                    }
                }
            }
            
            if (currentSlideIndex === 5) {
                const termsAccepted = document.getElementById('bookingTermsAccepted');
                if (!termsAccepted.checked) {
                    showMessage('Please confirm the booking details and accept the terms', 'error');
                    return false;
                }
            }
            
            return true;
        }
        
        function generateBookingSummary() {
            const summaryDiv = document.getElementById('bookingSummary');
            
            const locationRadio = document.querySelector('input[name="location"]:checked');
            let locationText = '';
            if (locationRadio.value === 'home') {
                locationText = `🏠 At Your Home<br><small class="text-gray-600">${currentFamily.address}</small>`;
            } else {
                const publicLocation = document.getElementById('publicLocationInput').value;
                locationText = `🌳 Public Location<br><small class="text-gray-600">${publicLocation}</small>`;
            }
            
            let summarySelectedChildren = [];
            if (bookingMode === 'multiple') {
                // For multiple dates, collect all unique children from sessions
                const allChildrenIndices = new Set();
                selectedSessions.forEach(session => {
                    if (session.children) {
                        session.children.forEach(childIndex => allChildrenIndices.add(childIndex));
                    }
                });
                summarySelectedChildren = Array.from(allChildrenIndices).map(index => currentFamily.children[index].name);
            } else if (bookingMode === 'multiday') {
                // For multi-day, use selected children or all if only one child
                if (currentFamily.children.length === 1) {
                    summarySelectedChildren = currentFamily.children.map(child => child.name);
                } else {
                    summarySelectedChildren = multiDaySelectedChildren.map(index => currentFamily.children[index].name);
                }
            } else {
                // For single date, use selected children or all if only one child
                if (currentFamily.children.length === 1) {
                    summarySelectedChildren = currentFamily.children.map(child => child.name);
                } else {
                    summarySelectedChildren = selectedChildren.map(index => currentFamily.children[index].name);
                }
            }
            
            let transportDetails = [];
            
            // Only check transport for single date mode
            if (bookingMode === 'single') {
                const needPickup = document.getElementById('needPickup');
                const needDropoff = document.getElementById('needDropoff');
                
                if (needPickup && needPickup.checked) {
                    const pickupLocation = document.getElementById('pickupLocation').value;
                    const pickupTime = document.getElementById('pickupTime').value;
                    transportDetails.push(`Pickup: ${pickupLocation} at ${pickupTime}`);
                }
                
                if (needDropoff && needDropoff.checked) {
                    const dropoffLocation = document.getElementById('dropoffLocation').value;
                    const dropoffTime = document.getElementById('dropoffTime').value;
                    transportDetails.push(`Drop-off: ${dropoffLocation} at ${dropoffTime}`);
                }
            }
            
            const useAlternate = document.getElementById('useAlternateContact').checked;
            let emergencyContact = currentFamily.emergencyContact;
            if (useAlternate) {
                const altName = document.getElementById('alternateContactName').value;
                const altPhone = document.getElementById('alternateContactPhone').value;
                emergencyContact = `${altName} (${altPhone})`;
            }
            
            const specialInstructions = document.getElementById('specialInstructions').value || 'None';
            
            let sessionDetailsHtml = '';
            
            if (bookingMode === 'multiday') {
                const endDate = new Date(multiDayStartDate);
                endDate.setDate(endDate.getDate() + multiDayDays - 1);
                const nights = multiDayDays - 1;
                
                sessionDetailsHtml = `
                    <div class="bg-gradient-to-r from-purple-50 to-pink-50 rounded-2xl p-6">
                        <h3 class="text-xl font-semibold mb-4" style="color: #18b79f;">🏠 Multi-Day Sitting Details</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="space-y-3">
                                <div>
                                    <div class="font-semibold text-gray-800">Sitting Period</div>
                                    <div class="text-gray-600">${multiDayStartDate.toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
                                    <div class="text-gray-600">to ${endDate.toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
                                </div>
                                <div>
                                    <div class="font-semibold text-gray-800">Location</div>
                                    <div class="text-gray-600">${locationText}</div>
                                </div>
                            </div>
                            <div class="space-y-3">
                                <div>
                                    <div class="font-semibold text-gray-800">Children (${summarySelectedChildren.length})</div>
                                    <div class="text-gray-600">${summarySelectedChildren.join(', ')}</div>
                                </div>
                                <div>
                                    <div class="font-semibold text-gray-800">Duration</div>
                                    <div class="text-2xl font-bold" style="color: #18b79f;">${multiDayDays} Days</div>
                                    <div class="text-sm text-gray-600">
                                        ${nights} night${nights !== 1 ? 's' : ''} with 24/7 care
                                        <br>🌙 Overnight supervision included
                                        ${transportDetails.length > 0 ? '<br>🚗 Transport included' : ''}
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else if (bookingMode === 'multiple') {
                sessionDetailsHtml = `
                    <div class="bg-gradient-to-r from-teal-50 to-blue-50 rounded-2xl p-6">
                        <h3 class="text-xl font-semibold mb-4" style="color: #18b79f;">📅 Multiple Sessions</h3>
                        <div class="space-y-4">
                            ${selectedSessions.map(session => `
                                <div class="bg-white rounded-lg p-4 border border-gray-200">
                                    <div class="flex justify-between items-start">
                                        <div>
                                            <div class="font-semibold text-gray-800">${session.date.toLocaleDateString('en-GB', { weekday: 'long', month: 'long', day: 'numeric' })}</div>
                                            <div class="text-gray-600">${session.time} (${session.duration === 'flexible' ? 'Flexible' : session.duration + 'h'})</div>
                                            <div class="text-sm text-gray-500">Children: ${session.children ? session.children.map(i => currentFamily.children[i].name).join(', ') : 'All'}</div>
                                        </div>
                                    </div>
                                </div>
                            `).join('')}
                        </div>
                        <div class="mt-4 pt-4 border-t border-gray-200">
                            <div class="font-semibold text-gray-800">Location</div>
                            <div class="text-gray-600">${locationText}</div>
                        </div>
                    </div>
                `;
            } else {
                sessionDetailsHtml = `
                    <div class="bg-gradient-to-r from-teal-50 to-blue-50 rounded-2xl p-6">
                        <h3 class="text-xl font-semibold mb-4" style="color: #18b79f;">📅 Session Details</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="space-y-3">
                                <div>
                                    <div class="font-semibold text-gray-800">Date & Time</div>
                                    <div class="text-gray-600">${selectedDate.toLocaleDateString('en-GB', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
                                    <div class="text-gray-600">${selectedTime} (${selectedDuration === 'flexible' ? 'Flexible duration' : selectedDuration + ' hours'})</div>
                                </div>
                                <div>
                                    <div class="font-semibold text-gray-800">Location</div>
                                    <div class="text-gray-600">${locationText}</div>
                                </div>
                            </div>
                            <div class="space-y-3">
                                <div>
                                    <div class="font-semibold text-gray-800">Children (${summarySelectedChildren.length})</div>
                                    <div class="text-gray-600">${summarySelectedChildren.join(', ')}</div>
                                </div>
                                <div>
                                    <div class="font-semibold text-gray-800">Duration</div>
                                    <div class="text-2xl font-bold" style="color: #18b79f;">${selectedDuration === 'flexible' ? 'Flexible' : selectedDuration + ' hours'}</div>
                                    <div class="text-sm text-gray-600">
                                        ${selectedDuration === 'flexible' ? 'Flexible duration' : selectedDuration + ' hour session'}
                                        ${transportDetails.length > 0 ? '<br>🚗 Transport included' : ''}
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            }
            
            summaryDiv.innerHTML = `
                ${sessionDetailsHtml}
                
                ${transportDetails.length > 0 ? `
                <div class="bg-gradient-to-r from-orange-50 to-yellow-50 rounded-2xl p-6">
                    <h3 class="text-lg font-semibold mb-3" style="color: #e88776;">🚗 Transport Services</h3>
                    <div class="space-y-2 text-sm">
                        ${transportDetails.map(detail => `<div>• ${detail}</div>`).join('')}
                    </div>
                </div>
                ` : ''}
                
                <div class="bg-gray-50 rounded-2xl p-6">
                    <h3 class="text-lg font-semibold mb-3" style="color: #e88776;">📋 Additional Details</h3>
                    <div class="space-y-2 text-sm">
                        <div><strong>Emergency Contact:</strong> ${emergencyContact}</div>
                        <div><strong>Special Instructions:</strong> ${specialInstructions}</div>
                        <div><strong>Parent:</strong> ${currentFamily.parentName} (${currentFamily.parentPhone})</div>
                    </div>
                </div>
            `;
        }
        
        function generateBookingReference() {
            const randomNum = Math.floor(Math.random() * 90000) + 10000;
            return `BK-${randomNum}`;
        }
        
        function submitBooking() {
            if (!validateCurrentSlide()) {
                return;
            }
            
            const submitBtn = document.querySelector('button[onclick="submitBooking()"]');
            const originalText = submitBtn.innerHTML;
            submitBtn.innerHTML = 'Processing Booking... ⏳';
            submitBtn.disabled = true;
            
            const bookingReference = generateBookingReference();
            
            // Collect booking data
            const bookingData = {
                reference: bookingReference,
                enrollmentNumber: currentFamily.enrollmentNumber,
                parentName: currentFamily.parentName,
                parentEmail: currentFamily.parentEmail,
                parentPhone: currentFamily.parentPhone,
                children: currentFamily.children,
                bookingMode: bookingMode,
                sessionDetails: getSessionDetails(),
                location: getLocationDetails(),
                transport: getTransportDetails(),
                emergencyContact: getEmergencyContactDetails(),
                specialInstructions: document.getElementById('specialInstructions').value || 'None',
                timestamp: new Date().toISOString()
            };
            
            // Initialize EmailJS
            emailjs.init("YOUR_PUBLIC_KEY"); // Replace with your EmailJS public key
            
            // Prepare email template parameters
            const templateParams = {
                booking_reference: bookingReference,
                parent_name: currentFamily.parentName,
                parent_email: currentFamily.parentEmail,
                parent_phone: currentFamily.parentPhone,
                children_names: currentFamily.children.map(child => child.name).join(', '),
                session_details: formatSessionDetailsForEmail(),
                location_details: formatLocationForEmail(),
                transport_details: formatTransportForEmail(),
                emergency_contact: getEmergencyContactDetails(),
                special_instructions: document.getElementById('specialInstructions').value || 'None',
                booking_date: new Date().toLocaleDateString('en-GB')
            };
            
            // Send confirmation email to parent
            emailjs.send("YOUR_SERVICE_ID", "YOUR_TEMPLATE_ID", templateParams)
                .then(function(response) {
                    console.log('Email sent successfully:', response);
                    
                    // Show success
                    document.getElementById('bookingReference').textContent = bookingReference;
                    currentSlideIndex = 6;
                    showSlide(6);
                    
                    // Log booking data
                    console.log('Booking submitted successfully:', bookingData);
                    
                    submitBtn.innerHTML = originalText;
                    submitBtn.disabled = false;
                }, function(error) {
                    console.error('Email sending failed:', error);
                    
                    // Still show success but note email issue
                    document.getElementById('bookingReference').textContent = bookingReference;
                    currentSlideIndex = 6;
                    showSlide(6);
                    
                    // Show a message about email
                    setTimeout(() => {
                        showMessage('Booking confirmed! Note: Confirmation email may be delayed.', 'info');
                    }, 1000);
                    
                    submitBtn.innerHTML = originalText;
                    submitBtn.disabled = false;
                });
        }
        
        function getSessionDetails() {
            if (bookingMode === 'multiday') {
                const endDate = new Date(multiDayStartDate);
                endDate.setDate(endDate.getDate() + multiDayDays - 1);
                const childrenNames = currentFamily.children.length === 1 
                    ? currentFamily.children.map(child => child.name)
                    : multiDaySelectedChildren.map(index => currentFamily.children[index].name);
                return {
                    type: 'multiday',
                    startDate: multiDayStartDate,
                    endDate: endDate,
                    days: multiDayDays,
                    nights: multiDayDays - 1,
                    children: childrenNames
                };
            } else if (bookingMode === 'multiple') {
                return {
                    type: 'multiple',
                    sessions: selectedSessions.map(session => ({
                        date: session.date,
                        time: session.time,
                        duration: session.duration,
                        children: session.children ? session.children.map(i => currentFamily.children[i].name) : []
                    }))
                };
            } else {
                const childrenNames = currentFamily.children.length === 1 
                    ? currentFamily.children.map(child => child.name)
                    : selectedChildren.map(index => currentFamily.children[index].name);
                return {
                    type: 'single',
                    date: selectedDate,
                    time: selectedTime,
                    duration: selectedDuration,
                    children: childrenNames
                };
            }
        }
        
        function getLocationDetails() {
            const locationRadio = document.querySelector('input[name="location"]:checked');
            if (locationRadio.value === 'home') {
                return {
                    type: 'home',
                    address: currentFamily.address
                };
            } else {
                return {
                    type: 'public',
                    location: document.getElementById('publicLocationInput').value
                };
            }
        }
        
        function getTransportDetails() {
            const transport = { pickup: null, dropoff: null };
            
            if (bookingMode === 'single') {
                const needPickup = document.getElementById('needPickup');
                const needDropoff = document.getElementById('needDropoff');
                
                if (needPickup && needPickup.checked) {
                    transport.pickup = {
                        location: document.getElementById('pickupLocation').value,
                        time: document.getElementById('pickupTime').value,
                        instructions: document.getElementById('pickupInstructions').value
                    };
                }
                
                if (needDropoff && needDropoff.checked) {
                    transport.dropoff = {
                        location: document.getElementById('dropoffLocation').value,
                        time: document.getElementById('dropoffTime').value,
                        instructions: document.getElementById('dropoffInstructions').value
                    };
                }
            }
            
            return transport;
        }
        
        function getEmergencyContactDetails() {
            const useAlternate = document.getElementById('useAlternateContact').checked;
            if (useAlternate) {
                const altName = document.getElementById('alternateContactName').value;
                const altPhone = document.getElementById('alternateContactPhone').value;
                return `${altName} (${altPhone})`;
            }
            return currentFamily.emergencyContact;
        }
        
        function formatSessionDetailsForEmail() {
            if (bookingMode === 'multiday') {
                const endDate = new Date(multiDayStartDate);
                endDate.setDate(endDate.getDate() + multiDayDays - 1);
                return `Multi-Day Sitting: ${multiDayStartDate.toLocaleDateString('en-GB')} to ${endDate.toLocaleDateString('en-GB')} (${multiDayDays} days, ${multiDayDays - 1} nights)`;
            } else if (bookingMode === 'multiple') {
                return selectedSessions.map(session => 
                    `${session.date.toLocaleDateString('en-GB')} at ${session.time} (${session.duration === 'flexible' ? 'Flexible' : session.duration + 'h'})`
                ).join(', ');
            } else {
                return `${selectedDate.toLocaleDateString('en-GB')} at ${selectedTime} (${selectedDuration === 'flexible' ? 'Flexible duration' : selectedDuration + ' hours'})`;
            }
        }
        
        function formatLocationForEmail() {
            const locationRadio = document.querySelector('input[name="location"]:checked');
            if (locationRadio.value === 'home') {
                return `At family home: ${currentFamily.address}`;
            } else {
                return `Public location: ${document.getElementById('publicLocationInput').value}`;
            }
        }
        
        function formatTransportForEmail() {
            const transport = getTransportDetails();
            const details = [];
            
            if (transport.pickup) {
                details.push(`Pickup: ${transport.pickup.location} at ${transport.pickup.time}`);
            }
            
            if (transport.dropoff) {
                details.push(`Drop-off: ${transport.dropoff.location} at ${transport.dropoff.time}`);
            }
            
            return details.length > 0 ? details.join(', ') : 'No transport required';
        }
        
        // Initialize the form
        function initializeForm() {
            currentSlideIndex = 1;
            showSlide(1);
            
            // Format enrollment number input - only allow numbers
            document.getElementById('enrollmentNumber').addEventListener('input', function(e) {
                let value = e.target.value.replace(/[^0-9]/g, '');
                if (value.length > 5) value = value.substring(0, 5);
                e.target.value = value;
            });
            
            // Allow Enter key to verify enrollment
            document.getElementById('enrollmentNumber').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    verifyEnrollment();
                }
            });
        }
        
        // Initialize when page loads
        initializeForm();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'98b6f6e781fbee7c',t:'MTc1OTk0MDQxNS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
