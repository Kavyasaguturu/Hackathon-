<!DOCTYPE html>  <html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Meeting Summary Popup</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <style>  
        /* Custom scrollbar for better aesthetics */  
        .custom-scrollbar::-webkit-scrollbar {  
            width: 8px;  
        }  
        .custom-scrollbar::-webkit-scrollbar-track {  
            background: #f1f1f1;  
            border-radius: 10px;  
        }  
        .custom-scrollbar::-webkit-scrollbar-thumb {  
            background: #888;  
            border-radius: 10px;  
        }  
        .custfom-scrollbar::-webkit-scrollbar-thumb:hover {  
            background: #555;  
        }  
        /* Ensure modal is above everything */  
        .modal-overlay {  
            z-index: 50;  
        }  
        .modal-box {  
            z-index: 51;  
        }  
    </style>  
</head>  
<body class="bg-gray-100 font-sans flex items-center justify-center min-h-screen p-4">  <button id="openSummaryBtn" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-lg shadow-md transition duration-150 ease-in-out">  
    View Meeting Summary  
</button>  

<div id="summaryModalOverlay" class="fixed inset-0 bg-black bg-opacity-60 hidden flex items-center justify-center p-4 modal-overlay transition-opacity duration-300 ease-in-out">  
    <div id="summaryModalBox" class="bg-white rounded-xl shadow-2xl w-full max-w-2xl transform transition-all duration-300 ease-in-out scale-95 opacity-0">  
        <div class="flex justify-between items-center p-6 border-b border-gray-200">  
            <h2 id="modalMeetingName" class="text-2xl font-bold text-gray-800">Project Phoenix - Q3 Review</h2>  
            <button id="closeSummaryBtn" class="text-gray-500 hover:text-gray-700 transition duration-150">  
                <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>  
            </button>  
        </div>  

        <div class="p-6 space-y-6 max-h-[70vh] overflow-y-auto custom-scrollbar">  
            <div class="grid grid-cols-1 md:grid-cols-2 gap-x-6 gap-y-4 text-sm">  
                <div>  
                    <span class="font-semibold text-gray-700">Date:</span>  
                    <span id="modalMeetingDate" class="text-gray-600 ml-2">May 30, 2025</span>  
                </div>  
                <div>  
                    <span class="font-semibold text-gray-700">Total Scheduled:</span>  
                    <span id="modalTotalScheduled" class="text-gray-600 ml-2">15 participants</span>  
                </div>  
                <div>  
                    <span class="font-semibold text-gray-700">Actual Attended:</span>  
                    <span id="modalActualHappened" class="text-gray-600 ml-2">12 participants</span>  
                </div>  
                <div>  
                    <span class="font-semibold text-gray-700">Status:</span>  
                    <span id="modalMeetingStatus" class="ml-2 px-3 py-1 text-xs font-semibold rounded-full bg-green-100 text-green-700">Completed</span>  
                    <!-- Example other statuses:  
                    <span class="ml-2 px-3 py-1 text-xs font-semibold rounded-full bg-yellow-100 text-yellow-700">In Progress</span>  
                    <span class="ml-2 px-3 py-1 text-xs font-semibold rounded-full bg-red-100 text-red-700">Cancelled</span>  
                    -->  
                </div>  
            </div>  

            <div>  
                <h3 class="text-xl font-semibold text-gray-800 mb-4 pt-4 border-t border-gray-200">Agenda Summary</h3>  
                <div id="modalAgendaSummaries" class="space-y-5">  
                    <div class="p-4 bg-gray-50 rounded-lg border border-gray-200">  
                        <h4 class="font-semibold text-gray-700 mb-1">1. Q2 Performance Review & Key Learnings</h4>  
                        <p class="text-sm text-gray-600 leading-relaxed">  
                            Discussed overall team performance in Q2, highlighting major achievements such as the successful launch of the Alpha version and exceeding user acquisition targets by 15%. Key learnings included the need for more robust testing on mobile devices and improving cross-departmental communication for feature releases.  
                        </p>  
                    </div>  
                    <div class="p-4 bg-gray-50 rounded-lg border border-gray-200">  
                        <h4 class="font-semibold text-gray-700 mb-1">2. Q3 Roadmap & Feature Prioritization</h4>  
                        <p class="text-sm text-gray-600 leading-relaxed">  
                            Outlined the proposed roadmap for Q3, focusing on user feedback integration, performance optimization, and two major new features: 'Collaborative Workspaces' and 'Advanced Analytics Dashboard'. After discussion, 'Advanced Analytics' was prioritized for initial rollout due to high customer demand.  
                        </p>  
                    </div>  
                    <div class="p-4 bg-gray-50 rounded-lg border border-gray-200">  
                        <h4 class="font-semibold text-gray-700 mb-1">3. Budget Allocation for Q3 Marketing</h4>  
                        <p class="text-sm text-gray-600 leading-relaxed">  
                            Reviewed the marketing budget proposal. Approved a 10% increase for digital advertising and content creation. A decision on allocating funds for a new PR agency was deferred pending further research on potential candidates.  
                        </p>  
                    </div>  
                     <div class="p-4 bg-gray-50 rounded-lg border border-gray-200">  
                        <h4 class="font-semibold text-gray-700 mb-1">4. Addressing User Feedback from Beta Program</h4>  
                        <p class="text-sm text-gray-600 leading-relaxed">  
                            Analyzed critical feedback from the beta program. Identified three main areas for immediate improvement: UI responsiveness on smaller screens, simplifying the onboarding process, and providing more comprehensive documentation. Action items were assigned to relevant teams.  
                        </p>  
                    </div>  
                </div>  
            </div>  
        </div>  

        <div class="flex justify-end items-center p-6 border-t border-gray-200 space-x-3">  
            <button id="closeSummaryBtnFooter" class="bg-gray-200 hover:bg-gray-300 text-gray-700 font-medium py-2 px-5 rounded-lg transition duration-150">  
                Close  
            </button>  
            <!-- <button class="bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-5 rounded-lg transition duration-150">  
                Print Summary  
            </button> -->  
        </div>  
    </div>  
</div>  

<script>  
    const openSummaryBtn = document.getElementById('openSummaryBtn');  
    const closeSummaryBtn = document.getElementById('closeSummaryBtn');  
    const closeSummaryBtnFooter = document.getElementById('closeSummaryBtnFooter');  
    const summaryModalOverlay = document.getElementById('summaryModalOverlay');  
    const summaryModalBox = document.getElementById('summaryModalBox');  

    // Dummy data (can be replaced with dynamic data)  
    const meetingData = {  
        name: "Quarterly Strategy Session - Q4 Planning",  
        date: "June 02, 2025",  
        totalScheduled: 20,  
        actualHappened: 18,  
        status: { text: "Completed", color: "green" }, // color can be green, yellow, red  
        agendas: [  
            {  
                title: "1. Review of Q3 Goals & Outcomes",  
                summary: "Assessed Q3 performance against set targets. Celebrated exceeding sales goals by 12% and identified areas for improvement in operational efficiency. Overall, a successful quarter with valuable insights gained."  
            },  
            {  
                title: "2. Presentation of Q4 Strategic Initiatives",  
                summary: "Introduced three core strategic initiatives for Q4: Market Expansion into APAC, Launch of New Product Line 'Synergy Pro', and Enhancing Customer Support Infrastructure. Each initiative's objectives and KPIs were detailed."  
            },  
            {  
                title: "3. Resource Allocation & Budgeting for Q4",  
                summary: "Discussed budget requirements for the proposed Q4 initiatives. Approved initial funding for market research in APAC and R&D for 'Synergy Pro'. Further budget reviews scheduled for mid-Q3."  
            },  
            {  
                title: "4. Open Forum & Q&A",  
                summary: "Team members raised questions regarding implementation timelines and potential challenges. Constructive feedback was gathered, and action points were noted for follow-up by department heads."  
            }  
        ]  
    };  

    function populateModal(data) {  
        document.getElementById('modalMeetingName').textContent = data.name;  
        document.getElementById('modalMeetingDate').textContent = data.date;  
        document.getElementById('modalTotalScheduled').textContent = `${data.totalScheduled} participants`;  
        document.getElementById('modalActualHappened').textContent = `${data.actualHappened} participants`;  
          
        const statusElement = document.getElementById('modalMeetingStatus');  
        statusElement.textContent = data.status.text;  
        statusElement.classList.remove('bg-green-100', 'text-green-700', 'bg-yellow-100', 'text-yellow-700', 'bg-red-100', 'text-red-700');  
        if (data.status.color === 'green') {  
            statusElement.classList.add('bg-green-100', 'text-green-700');  
        } else if (data.status.color === 'yellow') {  
            statusElement.classList.add('bg-yellow-100', 'text-yellow-700');  
        } else if (data.status.color === 'red') {  
            statusElement.classList.add('bg-red-100', 'text-red-700');  
        }  

        const agendasContainer = document.getElementById('modalAgendaSummaries');  
        agendasContainer.innerHTML = ''; // Clear existing agendas  
        data.agendas.forEach(agenda => {  
            const agendaDiv = document.createElement('div');  
            agendaDiv.className = 'p-4 bg-gray-50 rounded-lg border border-gray-200';  
            agendaDiv.innerHTML = `  
                <h4 class="font-semibold text-gray-700 mb-1">${agenda.title}</h4>  
                <p class="text-sm text-gray-600 leading-relaxed">${agenda.summary}</p>  
            `;  
            agendasContainer.appendChild(agendaDiv);  
        });  
    }  


    function openModal() {  
        // You can load dynamic data here if needed before showing  
        populateModal(meetingData); // Populate with dummy data or fetch real data  
        summaryModalOverlay.classList.remove('hidden');  
        setTimeout(() => { // Timeout for smooth transition  
            summaryModalOverlay.classList.remove('opacity-0');  
            summaryModalBox.classList.remove('scale-95', 'opacity-0');  
            summaryModalBox.classList.add('scale-100', 'opacity-100');  
        }, 10); // Small delay to allow display:flex to take effect before transition  
        document.body.style.overflow = 'hidden'; // Prevent background scroll  
    }  

    function closeModal() {  
        summaryModalBox.classList.remove('scale-100', 'opacity-100');  
        summaryModalBox.classList.add('scale-95', 'opacity-0');  
        summaryModalOverlay.classList.add('opacity-0');  
        setTimeout(() => {  
             summaryModalOverlay.classList.add('hidden');  
        }, 300); // Match transition duration  
        document.body.style.overflow = 'auto'; // Restore background scroll  
    }  

    openSummaryBtn.addEventListener('click', openModal);  
    closeSummaryBtn.addEventListener('click', closeModal);  
    closeSummaryBtnFooter.addEventListener('click', closeModal);  

    // Close modal on Escape key press  
    document.addEventListener('keydown', (event) => {  
        if (event.key === 'Escape' && !summaryModalOverlay.classList.contains('hidden')) {  
            closeModal();  
        }  
    });  

    // Close modal on overlay click (optional)  
    summaryModalOverlay.addEventListener('click', (event) => {  
        if (event.target === summaryModalOverlay) { // Check if the click is on the overlay itself  
            closeModal();  
        }  
    });  
</script>

</body>  
</html>  
