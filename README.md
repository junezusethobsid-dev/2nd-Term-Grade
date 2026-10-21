<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DepEd Student Grade Calculator</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f1f5f9;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block !important;
            }
            body {
                background-color: #ffffff !important;
            }
            .card-shadow {
                box-shadow: none !important;
                border: 1px solid #e2e8f0 !important;
            }
        }
    </style>
</head>
<body class="text-slate-800 min-h-screen pb-12">

    <!-- Header / Navbar -->
    <header class="bg-indigo-700 text-white shadow-md no-print mb-6">
        <div class="max-w-7xl mx-auto px-4 py-4 flex flex-wrap justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="bg-white/10 p-2 rounded-lg">
                    <i class="fa-solid fa-calculator text-2xl text-indigo-200"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">DepEd Grade Calculator</h1>
                    <p class="text-xs text-indigo-200">Written Works (20%) | Performance Tasks (50%) | Summative Tests (30%)</p>
                </div>
            </div>
            <div class="flex items-center gap-3">
                <button onclick="openTableModal()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3.5 py-2 rounded-lg text-sm font-medium transition flex items-center gap-2 border border-indigo-400/30">
                    <i class="fa-solid fa-table"></i> Transmutation Table
                </button>
                <button onclick="window.print()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3.5 py-2 rounded-lg text-sm font-medium transition flex items-center gap-2 shadow-sm">
                    <i class="fa-solid fa-print"></i> Print Report
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4">
        
        <!-- Printable Header Info -->
        <div class="hidden print-only mb-6 text-center border-b pb-4">
            <h1 class="text-2xl font-bold text-slate-900">STUDENT GRADE REPORT</h1>
            <p class="text-sm text-slate-500">Learner's Performance Assessment</p>
        </div>

        <!-- Student & Subject Information Card -->
        <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-5 mb-6 card-shadow">
            <h2 class="text-xs font-semibold text-indigo-600 uppercase tracking-wider mb-4 flex items-center gap-2">
                <i class="fa-solid fa-id-card"></i> Student Information
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Student Name</label>
                    <input type="text" id="studentName" placeholder="e.g. Juan Cruz" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Subject</label>
                    <input type="text" id="subjectName" placeholder="e.g. Mathematics" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Grade & Section / Quarter</label>
                    <input type="text" id="gradeSection" placeholder="e.g. Grade 10 - Newton / Q1" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none">
                </div>
            </div>
        </div>

        <!-- Summary Grade Dashboard Card -->
        <div class="bg-slate-900 text-white rounded-xl shadow-lg p-6 mb-6">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6 text-center divide-y md:divide-y-0 md:divide-x divide-slate-800">
                <div class="pt-2 md:pt-0">
                    <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Written Works (20%)</span>
                    <div id="wwSummary" class="text-2xl font-bold text-blue-400 mt-1">0.00%</div>
                    <span id="wwWeighted" class="text-xs text-slate-400">Weighted: 0.00</span>
                </div>
                <div class="pt-4 md:pt-0">
                    <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Performance Tasks (50%)</span>
                    <div id="ptSummary" class="text-2xl font-bold text-amber-400 mt-1">0.00%</div>
                    <span id="ptWeighted" class="text-xs text-slate-400">Weighted: 0.00</span>
                </div>
                <div class="pt-4 md:pt-0">
                    <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Summative Tests (30%)</span>
                    <div id="stSummary" class="text-2xl font-bold text-purple-400 mt-1">0.00%</div>
                    <span id="stWeighted" class="text-xs text-slate-400">Weighted: 0.00</span>
                </div>
                <div class="pt-4 md:pt-0 bg-slate-800/50 -m-6 p-6 rounded-b-xl md:rounded-r-xl md:rounded-bl-none flex flex-col justify-center items-center">
                    <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Transmuted Final Grade</span>
                    <div id="finalGradeDisplay" class="text-4xl font-extrabold text-emerald-400 mt-1">60</div>
                    <div class="flex items-center gap-2 mt-2">
                        <span id="initialGradeDisplay" class="text-xs text-slate-400">Initial: 0.00</span>
                        <span id="statusBadge" class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-red-500/20 text-red-300 border border-red-500/30">Did Not Meet Expectations</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Grade Input Component Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">

            <!-- 1. Written Works Component (20%) -->
            <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden flex flex-col card-shadow">
                <div class="bg-blue-50 border-b border-blue-100 p-4 flex justify-between items-center">
                    <div>
                        <h3 class="font-bold text-blue-900 flex items-center gap-2">
                            <i class="fa-solid fa-pen-to-square text-blue-600"></i> Written Works
                        </h3>
                        <p class="text-xs text-blue-700">Short Quizzes & Seatworks (20% Weight)</p>
                    </div>
                    <span id="wwScoreBadge" class="bg-blue-200 text-blue-800 text-xs font-bold px-2.5 py-1 rounded-full">0/0</span>
                </div>
                <div class="p-4 flex-grow">
                    <div class="text-xs font-semibold text-slate-500 grid grid-cols-12 gap-2 mb-2 px-1">
                        <span class="col-span-5">Activity / Quiz</span>
                        <span class="col-span-3 text-center">Score</span>
                        <span class="col-span-3 text-center">Total</span>
                        <span class="col-span-1 text-center no-print"></span>
                    </div>
                    <div id="wwContainer" class="space-y-2">
                        <!-- Rows injected via JS -->
                    </div>
                    <button onclick="addRow('ww')" class="mt-4 w-full py-2 border-2 border-dashed border-blue-200 text-blue-600 hover:bg-blue-50 rounded-lg text-xs font-semibold transition flex items-center justify-center gap-1.5 no-print">
                        <i class="fa-solid fa-plus"></i> Add Item
                    </button>
                </div>
                <div class="bg-slate-50 p-3 border-t border-slate-100 text-xs flex justify-between items-center">
                    <span class="text-slate-600 font-medium">Percentage Total:</span>
                    <span id="wwPct" class="font-bold text-blue-700 text-sm">0.00%</span>
                </div>
            </div>

            <!-- 2. Performance Tasks Component (50%) -->
            <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden flex flex-col card-shadow">
                <div class="bg-amber-50 border-b border-amber-100 p-4 flex justify-between items-center">
                    <div>
                        <h3 class="font-bold text-amber-900 flex items-center gap-2">
                            <i class="fa-solid fa-list-check text-amber-600"></i> Performance Tasks
                        </h3>
                        <p class="text-xs text-amber-700">Projects, Labs & Rubrics (50% Weight)</p>
                    </div>
                    <span id="ptScoreBadge" class="bg-amber-200 text-amber-800 text-xs font-bold px-2.5 py-1 rounded-full">0/0</span>
                </div>
                <div class="p-4 flex-grow">
                    <div class="text-xs font-semibold text-slate-500 grid grid-cols-12 gap-2 mb-2 px-1">
                        <span class="col-span-5">Task Name</span>
                        <span class="col-span-3 text-center">Score</span>
                        <span class="col-span-3 text-center">Total</span>
                        <span class="col-span-1 text-center no-print"></span>
                    </div>
                    <div id="ptContainer" class="space-y-2">
                        <!-- Rows injected via JS -->
                    </div>
                    <button onclick="addRow('pt')" class="mt-4 w-full py-2 border-2 border-dashed border-amber-200 text-amber-600 hover:bg-amber-50 rounded-lg text-xs font-semibold transition flex items-center justify-center gap-1.5 no-print">
                        <i class="fa-solid fa-plus"></i> Add Task
                    </button>
                </div>
                <div class="bg-slate-50 p-3 border-t border-slate-100 text-xs flex justify-between items-center">
                    <span class="text-slate-600 font-medium">Percentage Total:</span>
                    <span id="ptPct" class="font-bold text-amber-700 text-sm">0.00%</span>
                </div>
            </div>

            <!-- 3. Summative Tests Component (30%) -->
            <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden flex flex-col card-shadow">
                <div class="bg-purple-50 border-b border-purple-100 p-4 flex justify-between items-center">
                    <div>
                        <h3 class="font-bold text-purple-900 flex items-center gap-2">
                            <i class="fa-solid fa-file-lines text-purple-600"></i> Summative Tests
                        </h3>
                        <p class="text-xs text-purple-700">Unit & Quarterly Exams (30% Weight)</p>
                    </div>
                    <span id="stScoreBadge" class="bg-purple-200 text-purple-800 text-xs font-bold px-2.5 py-1 rounded-full">0/0</span>
                </div>
                <div class="p-4 flex-grow">
                    <div class="text-xs font-semibold text-slate-500 grid grid-cols-12 gap-2 mb-2 px-1">
                        <span class="col-span-5">Test Title</span>
                        <span class="col-span-3 text-center">Score</span>
                        <span class="col-span-3 text-center">Total</span>
                        <span class="col-span-1 text-center no-print"></span>
                    </div>
                    <div id="stContainer" class="space-y-2">
                        <!-- Rows injected via JS -->
                    </div>
                    <button onclick="addRow('st')" class="mt-4 w-full py-2 border-2 border-dashed border-purple-200 text-purple-600 hover:bg-purple-50 rounded-lg text-xs font-semibold transition flex items-center justify-center gap-1.5 no-print">
                        <i class="fa-solid fa-plus"></i> Add Test
                    </button>
                </div>
                <div class="bg-slate-50 p-3 border-t border-slate-100 text-xs flex justify-between items-center">
                    <span class="text-slate-600 font-medium">Percentage Total:</span>
                    <span id="stPct" class="font-bold text-purple-700 text-sm">0.00%</span>
                </div>
            </div>

        </div>

    </main>

    <!-- Transmutation Table Modal -->
    <div id="tableModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-2xl shadow-xl max-w-2xl w-full max-h-[90vh] flex flex-col overflow-hidden">
            <div class="p-4 bg-indigo-700 text-white flex justify-between items-center">
                <h3 class="font-bold flex items-center gap-2">
                    <i class="fa-solid fa-table"></i> DepEd Transmutation Table
                </h3>
                <button onclick="closeTableModal()" class="text-indigo-200 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <div class="p-4 overflow-y-auto flex-grow text-xs">
                <div class="grid grid-cols-2 gap-4">
                    <table class="w-full text-left border-collapse border border-slate-200">
                        <thead>
                            <tr class="bg-slate-100 border-b border-slate-200 text-slate-700">
                                <th class="p-2 border-r">Initial Grade</th>
                                <th class="p-2 text-center">Transmuted</th>
                            </tr>
                        </thead>
                        <tbody id="tableCol1" class="divide-y divide-slate-100"></tbody>
                    </table>
                    <table class="w-full text-left border-collapse border border-slate-200">
                        <thead>
                            <tr class="bg-slate-100 border-b border-slate-200 text-slate-700">
                                <th class="p-2 border-r">Initial Grade</th>
                                <th class="p-2 text-center">Transmuted</th>
                            </tr>
                        </thead>
                        <tbody id="tableCol2" class="divide-y divide-slate-100"></tbody>
                    </table>
                </div>
            </div>
            <div class="p-3 bg-slate-50 border-t border-slate-200 text-right">
                <button onclick="closeTableModal()" class="px-4 py-1.5 bg-slate-700 hover:bg-slate-800 text-white rounded-lg text-xs font-medium">Close</button>
            </div>
        </div>
    </div>

    <!-- Script Logic -->
    <script>
        // Transmutation Mapping Data Table
        const transmutationTable = [
            { min: 99.50, max: 100.00, grade: 100 },
            { min: 97.50, max: 99.49, grade: 99 },
            { min: 96.00, max: 97.49, grade: 98 },
            { min: 95.00, max: 95.99, grade: 97 },
            { min: 94.00, max: 94.99, grade: 96 },
            { min: 93.00, max: 93.99, grade: 95 },
            { min: 92.00, max: 92.99, grade: 94 },
            { min: 91.00, max: 91.99, grade: 93 },
            { min: 90.00, max: 90.99, grade: 92 },
            { min: 89.00, max: 89.99, grade: 91 },
            { min: 88.00, max: 88.99, grade: 90 },
            { min: 87.00, max: 87.99, grade: 89 },
            { min: 86.00, max: 86.99, grade: 88 },
            { min: 85.00, max: 85.99, grade: 87 },
            { min: 84.00, max: 84.99, grade: 86 },
            { min: 83.00, max: 83.99, grade: 85 },
            { min: 82.00, max: 82.99, grade: 84 },
            { min: 81.00, max: 81.99, grade: 83 },
            { min: 80.00, max: 80.99, grade: 82 },
            { min: 79.00, max: 79.99, grade: 81 },
            { min: 78.00, max: 78.99, grade: 80 },
            { min: 77.00, max: 77.99, grade: 79 },
            { min: 76.00, max: 76.99, grade: 78 },
            { min: 75.00, max: 75.99, grade: 77 },
            { min: 73.00, max: 74.99, grade: 76 },
            { min: 70.00, max: 72.99, grade: 75 },
            { min: 68.00, max: 69.99, grade: 74 },
            { min: 66.00, max: 67.99, grade: 73 },
            { min: 64.00, max: 65.99, grade: 72 },
            { min: 62.00, max: 63.99, grade: 71 },
            { min: 60.00, max: 61.99, grade: 70 },
            { min: 58.00, max: 59.99, grade: 69 },
            { min: 56.00, max: 57.99, grade: 68 },
            { min: 54.00, max: 55.99, grade: 67 },
            { min: 52.00, max: 53.99, grade: 66 },
            { min: 50.00, max: 51.99, grade: 65 },
            { min: 48.00, max: 49.99, grade: 64 },
            { min: 46.00, max: 47.99, grade: 63 },
            { min: 43.00, max: 45.99, grade: 62 },
            { min: 40.00, max: 42.99, grade: 61 },
            { min: 25.00, max: 39.99, grade: 60 },
            { min: 0.00,  max: 24.99, grade: 60 }
        ];

        // Store counts for row indices
        let counts = { ww: 0, pt: 0, st: 0 };

        // Initialize component tables with default rows
        window.onload = function() {
            // Initial rows for Written Works
            addRow('ww', 'Quiz 1', 18, 20);
            addRow('ww', 'Seatwork 1', 10, 10);
            addRow('ww', 'Quiz 2', 15, 20);

            // Initial rows for Performance Tasks
            addRow('pt', 'Performance Task 1', 45, 50);
            addRow('pt', 'Project 1', 48, 50);

            // Initial rows for Summative Tests
            addRow('st', 'Unit Test 1', 40, 50);

            populateTransmutationTableModal();
            calculateAll();
        };

        // Dynamic Row Addition
        function addRow(type, label = '', score = '', total = '') {
            counts[type]++;
            const id = `${type}_${counts[type]}`;
            const container = document.getElementById(`${type}Container`);

            const placeholderText = type === 'ww' ? `Item ${counts[type]}` :
                                   type === 'pt' ? `Task ${counts[type]}` : `Test ${counts[type]}`;

            const row = document.createElement('div');
            row.id = `row_${id}`;
            row.className = 'grid grid-cols-12 gap-2 items-center';
            row.innerHTML = `
                <div class="col-span-5">
                    <input type="text" value="${label}" placeholder="${placeholderText}" 
                        class="w-full px-2.5 py-1.5 border border-slate-200 rounded-lg text-xs focus:ring-1 focus:ring-indigo-500 outline-none">
                </div>
                <div class="col-span-3">
                    <input type="number" min="0" value="${score}" placeholder="0" oninput="calculateAll()"
                        class="score-input w-full px-2 py-1.5 border border-slate-200 rounded-lg text-xs text-center font-medium focus:ring-1 focus:ring-indigo-500 outline-none">
                </div>
                <div class="col-span-3">
                    <input type="number" min="1" value="${total}" placeholder="0" oninput="calculateAll()"
                        class="total-input w-full px-2 py-1.5 border border-slate-200 rounded-lg text-xs text-center font-medium focus:ring-1 focus:ring-indigo-500 outline-none">
                </div>
                <div class="col-span-1 text-center no-print">
                    <button onclick="removeRow('row_${id}')" class="text-slate-400 hover:text-red-500 p-1 text-xs transition">
                        <i class="fa-solid fa-trash-can"></i>
                    </button>
                </div>
            `;
            container.appendChild(row);
            calculateAll();
        }

        // Remove Row Function
        function removeRow(rowId) {
            const el = document.getElementById(rowId);
            if (el) {
                el.remove();
                calculateAll();
            }
        }

        // Calculate Grade Logic
        function calculateAll() {
            const ww = calculateComponent('ww');
            const pt = calculateComponent('pt');
            const st = calculateComponent('st');

            // Weighted Component Computation
            const wwWeighted = ww.pct * 0.20;
            const ptWeighted = pt.pct * 0.50;
            const stWeighted = st.pct * 0.30;

            const initialGrade = wwWeighted + ptWeighted + stWeighted;
            const finalGrade = transmute(initialGrade);

            // Update UI Displays for Components
            updateComponentUI('ww', ww, wwWeighted);
            updateComponentUI('pt', pt, ptWeighted);
            updateComponentUI('st', st, stWeighted);

            // Update Overall Summary Dashboard
            document.getElementById('initialGradeDisplay').innerText = `Initial Grade: ${initialGrade.toFixed(2)}`;
            document.getElementById('finalGradeDisplay').innerText = finalGrade;

            // Status Badge Formatting
            const badge = document.getElementById('statusBadge');
            if (finalGrade >= 75) {
                badge.innerText = "Passed";
                badge.className = "px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
            } else {
                badge.innerText = "Did Not Meet Expectations";
                badge.className = "px-2.5 py-0.5 rounded-full text-xs font-semibold bg-red-500/20 text-red-300 border border-red-500/30";
            }
        }

        // Compute Component Raw Totals
        function calculateComponent(type) {
            const container = document.getElementById(`${type}Container`);
            const rows = container.querySelectorAll('.grid');

            let totalScore = 0;
            let totalPossible = 0;

            rows.forEach(row => {
                const scoreVal = parseFloat(row.querySelector('.score-input').value) || 0;
                const totalVal = parseFloat(row.querySelector('.total-input').value) || 0;

                totalScore += scoreVal;
                totalPossible += totalVal;
            });

            const pct = totalPossible > 0 ? (totalScore / totalPossible) * 100 : 0;
            return { totalScore, totalPossible, pct };
        }

        // Update UI Elements per Category
        function updateComponentUI(type, data, weightedVal) {
            document.getElementById(`${type}ScoreBadge`).innerText = `${data.totalScore}/${data.totalPossible}`;
            document.getElementById(`${type}Pct`).innerText = `${data.pct.toFixed(2)}%`;
            document.getElementById(`${type}Summary`).innerText = `${data.pct.toFixed(2)}%`;
            document.getElementById(`${type}Weighted`).innerText = `Weighted: ${weightedVal.toFixed(2)}`;
        }

        // Transmutation Mapping Lookup
        function transmute(initial) {
            // Clamp initial grade to 0-100 range
            const roundedInitial = Math.min(100, Math.max(0, initial));

            for (let item of transmutationTable) {
                if (roundedInitial >= item.min && roundedInitial <= item.max) {
                    return item.grade;
                }
            }
            return 60; // Default minimum
        }

        // Populate Modal Transmutation Table
        function populateTransmutationTableModal() {
            const col1 = document.getElementById('tableCol1');
            const col2 = document.getElementById('tableCol2');

            col1.innerHTML = '';
            col2.innerHTML = '';

            const half = Math.ceil(transmutationTable.length / 2);

            transmutationTable.forEach((row, index) => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50";
                tr.innerHTML = `
                    <td class="p-2 border-r font-mono text-slate-600">${row.min.toFixed(2)} - ${row.max.toFixed(2)}</td>
                    <td class="p-2 text-center font-bold text-indigo-700">${row.grade}</td>
                `;

                if (index < half) {
                    col1.appendChild(tr);
                } else {
                    col2.appendChild(tr);
                }
            });
        }

        // Modal Controls
        function openTableModal() {
            document.getElementById('tableModal').classList.remove('hidden');
        }

        function closeTableModal() {
            document.getElementById('tableModal').classList.add('hidden');
        }
    </script>
</body>
</html>
