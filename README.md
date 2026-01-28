<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Civil Engineering Strategic Reviewer</title>
    
    <!-- React & ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    
    <!-- Babel for JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- Google Fonts (Inter) -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800">

    <div id="root"></div>

    <script type="text/babel">
        const { useState, useMemo, useEffect } = React;

        // --- ICONS (Inline SVGs to remove external dependencies) ---
        const Icon = ({ path, size = 24, className = "" }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
                {path}
            </svg>
        );

        const Icons = {
            BookOpen: (props) => <Icon {...props} path={<><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></>} />,
            CheckCircle: (props) => <Icon {...props} path={<><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></>} />,
            XCircle: (props) => <Icon {...props} path={<><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></>} />,
            ChevronRight: (props) => <Icon {...props} path={<polyline points="9 18 15 12 9 6"/>} />,
            RotateCcw: (props) => <Icon {...props} path={<><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/></>} />,
            Award: (props) => <Icon {...props} path={<><circle cx="12" cy="8" r="7"/><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"/></>} />,
            Calculator: (props) => <Icon {...props} path={<><rect x="4" y="2" width="16" height="20" rx="2"/><line x1="8" y1="6" x2="16" y2="6"/><line x1="16" y1="14" x2="16" y2="14"/><line x1="12" y1="14" x2="12" y2="14"/><line x1="8" y1="14" x2="8" y2="14"/><line x1="16" y1="18" x2="16" y2="18"/><line x1="12" y1="18" x2="12" y2="18"/><line x1="8" y1="18" x2="8" y2="18"/></>} />,
            Lightbulb: (props) => <Icon {...props} path={<><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-1 1.5-2 1.5-3.5 0-3-2.5-5.5-5.5-5.5S7 5 7 8c0 1.5.5 2.5 1.5 3.5.8.8 1.3 1.5 1.5 2.5"/><path d="M9 18h6"/><path d="M10 22h4"/></>} />,
            Settings: (props) => <Icon {...props} path={<><path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.09a2 2 0 0 1-1-1.74v-.47a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.39a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"/><circle cx="12" cy="12" r="3"/></>} />,
            Layers: (props) => <Icon {...props} path={<><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></>} />,
            ListFilter: (props) => <Icon {...props} path={<><line x1="21" y1="4" x2="3" y2="4"/><line x1="21" y1="10" x2="3" y2="10"/><line x1="21" y1="16" x2="3" y2="16"/><line x1="21" y1="22" x2="3" y2="22"/></>} />, // Simplification for ease
            PlayCircle: (props) => <Icon {...props} path={<><circle cx="12" cy="12" r="10"/><polygon points="10 8 16 12 10 16 10 8"/></>} />
        };

        // --- QUESTION DATA ---
        const allQuestions = [
            // --- ENGINEERING MECHANICS ---
            {
                id: 1,
                category: "Engineering Mechanics",
                question: "A 100 kg weight hangs from a rope that is tied to two other ropes attached to a ceiling. The two ceiling ropes make angles of 45° and 60° with the horizontal. Which theorem is best used to solve for the tension in the ropes?",
                options: ["Varignon's Theorem", "Lami's Theorem", "Parallel Axis Theorem", "Castigliano's Theorem"],
                correct: 1,
                solution: "SHORTCUT: Lami's Theorem is the fastest way to solve for 3 concurrent forces in equilibrium. F1/sin(α) = F2/sin(β) = F3/sin(γ)."
            },
            {
                id: 2,
                category: "Engineering Mechanics",
                question: "A simple beam 6 meters long carries a concentrated load of 30 kN at a distance of 2 meters from the left support. What is the vertical reaction at the left support?",
                options: ["10 kN", "15 kN", "20 kN", "30 kN"],
                correct: 2,
                solution: "FORMULA: Sum Moments at Right Support = 0. \nR_left(6) = 30(4) \n6 R_left = 120 \nR_left = 20 kN."
            },
            {
                id: 3,
                category: "Engineering Mechanics",
                question: "A 500 N ladder rests against a smooth vertical wall and a rough horizontal floor. The ladder makes an angle of 60° with the floor. What is the vertical reaction at the floor?",
                options: ["250 N", "433 N", "500 N", "0 N"],
                correct: 2,
                solution: "CONCEPT: Sum Forces Vertical = 0. \nThe wall is smooth (no friction), so it has no vertical component. The floor supports the entire weight. \nRy = W = 500 N."
            },
            {
                id: 4,
                category: "Engineering Mechanics",
                question: "A block of weight W = 500 N rests on a horizontal surface. The coefficient of static friction is 0.40. What horizontal force P is required to just start the block moving?",
                options: ["150 N", "200 N", "250 N", "500 N"],
                correct: 1,
                solution: "FORMULA: F_max = μN \nSince it's horizontal, Normal Force N = W = 500. \nP = 0.40 * 500 = 200 N."
            },
            {
                id: 5,
                category: "Engineering Mechanics",
                question: "A 100 N box is placed on a plane inclined at 30° to the horizontal. The coefficient of static friction is 0.30. What is the computed friction capacity available?",
                options: ["25.98 N", "30.00 N", "50.00 N", "86.60 N"],
                correct: 0,
                solution: "FORMULA: f_max = μN \nN = W cos(θ) = 100 cos(30°) = 86.6 N \nf_max = 0.30 * 86.6 = 25.98 N."
            },
            {
                id: 6,
                category: "Engineering Mechanics",
                question: "Which of the following statements about friction is generally TRUE in rigid body mechanics?",
                options: ["Friction acts perpendicular to contact", "Static friction is generally less than kinetic", "The angle of friction is the angle between Normal and Resultant", "Friction depends heavily on contact area"],
                correct: 2,
                solution: "DEFINITION: The Angle of Friction (φ) is the angle between the Normal force and the Resultant reaction. tan(φ) = μ."
            },
            {
                id: 7,
                category: "Engineering Mechanics",
                question: "A vertical pole is supported by three cables. The tensions are concurrent at the top. To solve for cable forces, sum moments about:",
                options: ["The top of the pole", "The base of the pole", "The center of gravity", "Any point on a cable"],
                correct: 1,
                solution: "STRATEGY: Summing moments about the BASE eliminates the reactions at the base (which you likely don't know yet), allowing you to relate the cable tensions to the external loads."
            },
            {
                id: 8,
                category: "Engineering Mechanics",
                question: "In a 3D vector system, if a force passes through A(0,0,0) and B(3,4,12), what is the direction cosine with respect to the z-axis (cos γ)?",
                options: ["3/13", "4/13", "12/13", "1/13"],
                correct: 2,
                solution: "FORMULA: Distance d = sqrt(x² + y² + z²) = sqrt(3² + 4² + 12²) = sqrt(169) = 13. \ncos(γ) = z / d = 12 / 13."
            },
            {
                id: 9,
                category: "Engineering Mechanics",
                question: "Force F = 100i - 200j + 300k N acts at the origin. What is the magnitude?",
                options: ["374.17 N", "600 N", "200 N", "450.5 N"],
                correct: 0,
                solution: "FORMULA: Resultant = sqrt(Fx² + Fy² + Fz²) \nR = sqrt(100² + (-200)² + 300²) = sqrt(140,000) ≈ 374.17 N."
            },
            // --- STRENGTH OF MATERIALS ---
            {
                id: 10,
                category: "Strength of Materials",
                question: "The property of a material that allows it to be drawn into fine wires is called:",
                options: ["Malleability", "Ductility", "Brittleness", "Elasticity"],
                correct: 1,
                solution: "MNEMONIC: Ductile = Drawn (Wire). Malleable = Mallet (Hammered into sheets)."
            },
            {
                id: 11,
                category: "Strength of Materials",
                question: "The ratio of lateral strain to longitudinal strain within the elastic limit is:",
                options: ["Young's Modulus", "Shear Modulus", "Poisson's Ratio", "Bulk Modulus"],
                correct: 2,
                solution: "DEFINITION: ν (Nu) = - (Lateral Strain / Longitudinal Strain). This is Poisson's Ratio."
            },
            {
                id: 12,
                category: "Strength of Materials",
                question: "Which material property represents the area under the stress-strain curve up to the fracture point?",
                options: ["Resilience", "Toughness", "Proportional Limit", "Yield Strength"],
                correct: 1,
                solution: "CONCEPT: Resilience is area up to Elastic Limit. Toughness is area up to Fracture (Total energy absorbed)."
            },
            {
                id: 13,
                category: "Strength of Materials",
                question: "A steel rod with area 200 mm² is subjected to 30 kN tension. What is the axial stress?",
                options: ["150 MPa", "15 MPa", "150 kPa", "600 MPa"],
                correct: 0,
                solution: "FORMULA: σ = P/A \nP = 30,000 N, A = 200 mm². \nσ = 30000/200 = 150 N/mm² = 150 MPa."
            },
            {
                id: 14,
                category: "Strength of Materials",
                question: "A bolt with diameter 20 mm is subjected to a single shear force of 40 kN. Average shear stress?",
                options: ["63.66 MPa", "127.32 MPa", "31.83 MPa", "200 MPa"],
                correct: 1,
                solution: "FORMULA: τ = V/A \nArea = π(20)²/4 = 314.16 mm². \nτ = 40,000 N / 314.16 mm² = 127.32 MPa."
            },
            {
                id: 15,
                category: "Strength of Materials",
                question: "A 10m rail is heated by 30°C and prevented from expanding. What stress is induced?",
                options: ["Tensile Stress", "Compressive Stress", "Shear Stress", "Bearing Stress"],
                correct: 1,
                solution: "CONCEPT: If a material wants to expand (due to heat) but is prevented by supports, the supports must push back, creating COMPRESSIVE stress."
            },
            // --- THEORY OF STRUCTURES ---
            {
                id: 16,
                category: "Theory of Structures",
                question: "Degree of static indeterminacy for a propped cantilever beam (fixed one end, roller other)?",
                options: ["Determinate", "1st Degree", "2nd Degree", "3rd Degree"],
                correct: 1,
                solution: "FORMULA: r - 3 (for beams). \nReactions = 3 (Fixed) + 1 (Roller) = 4. \nIndeterminacy = 4 - 3 = 1."
            },
            {
                id: 17,
                category: "Theory of Structures",
                question: "Degree of indeterminacy for a fixed-ended beam (fixed at both ends).",
                options: ["1st Degree", "2nd Degree", "3rd Degree", "4th Degree"],
                correct: 2,
                solution: "FORMULA: r - 3. \nReactions = 3 (Left) + 3 (Right) = 6. \nIndeterminacy = 6 - 3 = 3."
            },
            {
                id: 18,
                category: "Theory of Structures",
                question: "Plane truss: 5 joints, 7 members, 3 reaction components. Is it determinate?",
                options: ["Unstable", "Determinate", "Indeterminate 1st", "Indeterminate 2nd"],
                correct: 1,
                solution: "FORMULA: m + r = 2j \nLeft Side: 7 + 3 = 10. \nRight Side: 2(5) = 10. \n10 = 10, so it is Statically Determinate."
            },
            {
                id: 19,
                category: "Theory of Structures",
                question: "Simply supported beam with uniform load w over span L. Vertical reaction at support?",
                options: ["wL", "wL/2", "wL^2/8", "wL/4"],
                correct: 1,
                solution: "SHORTCUT: Due to symmetry, the total load (wL) is shared equally by the two supports. R = wL/2."
            },
            {
                id: 20,
                category: "Theory of Structures",
                question: "Where does the maximum bending moment occur in a simply supported beam with uniform load?",
                options: ["At supports", "At inflection point", "Where shear is zero", "Where shear is max"],
                correct: 2,
                solution: "CONCEPT: Max Moment always occurs where the Shear Diagram crosses zero (V=0)."
            },
            {
                id: 21,
                category: "Theory of Structures",
                question: "Cantilever beam length L, concentrated load P at free end. Max moment?",
                options: ["PL/4", "PL/2", "PL", "PL^2/8"],
                correct: 2,
                solution: "FORMULA: M = Force × Distance. \nAt the fixed support, distance is L. \nM = P × L."
            },
            {
                id: 22,
                category: "Theory of Structures",
                question: "The influence line for the reaction at the left support of a simply supported beam is:",
                options: ["Triangle, height 1 at left", "Triangle, height 1 at center", "Rectangle", "Parabola"],
                correct: 0,
                solution: "VISUALIZATION: If you put a unit load at the left support, Reaction = 1. If at right support, Reaction = 0. Connect them: A triangle with ordinate 1.0 at the left."
            },
            {
                id: 23,
                category: "Theory of Structures",
                question: "To maximize positive moment at the center of a continuous beam (3 spans), where do you place live load?",
                options: ["Spans 1 & 2", "Spans 1 & 3", "Span 2 only", "All spans"],
                correct: 1,
                solution: "PATTERN LOADING: For Max Positive Moment in a span, load that span and every alternate span. If checking span 1: Load 1 & 3. If checking span 2: Load 2."
            },
            {
                id: 24,
                category: "Theory of Structures",
                question: "To obtain maximum negative moment at a support in a continuous beam, load:",
                options: ["Two spans adjacent to support", "Alternate spans", "Furthest span", "Longer span"],
                correct: 0,
                solution: "PATTERN LOADING: To break the beam (max negative) over a support, load the two spans touching that support."
            },
            {
                id: 25,
                category: "Theory of Structures",
                question: "In frame analysis, a 'moment connection' transfers:",
                options: ["Axial only", "Shear only", "Moment, Shear, Axial", "Moment only"],
                correct: 2,
                solution: "DEFINITION: A rigid or moment connection restricts all relative motion: x, y, and rotation. Therefore it transfers Axial, Shear, and Moment."
            },
            {
                id: 26,
                category: "Theory of Structures",
                question: "Zero Force Member Rule: 2 non-collinear members meet at a joint with no external load. result?",
                options: ["Both in tension", "Both in compression", "Both are zero", "One is zero"],
                correct: 2,
                solution: "SHORTCUT: If only 2 members meet at an unloaded joint and they aren't in a straight line, neither can push back against the other. Both must be zero."
            },
            {
                id: 27,
                category: "Theory of Structures",
                question: "Best method for finding deflection of a truss at a single specific joint?",
                options: ["Slope Deflection", "Moment Distribution", "Unit Load (Virtual Work)", "Three-Moment"],
                correct: 2,
                solution: "CONCEPT: Virtual Work (Unit Load Method) is specifically designed to find deformation at ONE point. Methods like Slope Deflection find end moments."
            },
            // --- REINFORCED CONCRETE ---
            {
                id: 28,
                category: "Reinforced Concrete",
                question: "The 'Whitney Stress Block' approximates concrete compression as a rectangle with depth:",
                options: ["c", "0.85c (or β1 c)", "d", "0.5d"],
                correct: 1,
                solution: "CODE: a = β1 * c. The depth of the equivalent rectangular block 'a' is a fraction of the true neutral axis 'c'."
            },
            {
                id: 29,
                category: "Reinforced Concrete",
                question: "If tension steel strain εt is 0.005 or greater, the section is:",
                options: ["Compression-controlled", "Transition", "Tension-controlled", "Balanced"],
                correct: 2,
                solution: "CODE LIMIT: εt ≥ 0.005 means the steel is yielding significantly before concrete crushes. This is 'Tension-Controlled' (Ductile)."
            },
            {
                id: 30,
                category: "Reinforced Concrete",
                question: "Strength reduction factor (φ) for tension-controlled RC beam in flexure:",
                options: ["0.65", "0.75", "0.85", "0.90"],
                correct: 3,
                solution: "CODE: For ductile, tension-controlled flexure (beams), φ = 0.90."
            },
            {
                id: 31,
                category: "Prestressed Concrete",
                question: "Loss of prestress due to shortening of concrete immediately upon transfer is:",
                options: ["Creep", "Shrinkage", "Elastic Shortening", "Relaxation"],
                correct: 2,
                solution: "CONCEPT: Elastic Shortening happens instantly when the force is applied. Creep/Shrinkage/Relaxation happen over time."
            },
            {
                id: 32,
                category: "Prestressed Concrete",
                question: "Which loading stage usually governs the design of concrete stresses at the extreme fiber?",
                options: ["Initial / Transfer", "Service", "Ultimate", "Cracking"],
                correct: 0,
                solution: "DESIGN: The 'Transfer' stage is critical because the concrete is often younger (weaker) and the prestress force is at its maximum (no losses yet)."
            },
            {
                id: 33,
                category: "Prestressed Concrete",
                question: "Primary purpose of high-strength steel in prestressed concrete?",
                options: ["Increase ductility", "Reduce cost", "Overcome losses", "Increase bond"],
                correct: 2,
                solution: "REASONING: Prestress losses (shrinkage/creep) can be 200+ MPa. If you used normal steel (Fy=400), you'd lose half your force. High strength (Fy=1860) ensures clamping force remains."
            },
            // --- STEEL DESIGN ---
            {
                id: 34,
                category: "Steel Design",
                question: "For a beam to reach full Plastic Moment (Mp) without buckling, section must be:",
                options: ["Compact & Laterally Supported", "Non-compact & Supported", "Slender", "Built-up"],
                correct: 0,
                solution: "CODE: To achieve Mp, we must prevent Local Buckling (Compact) and Lateral Torsional Buckling (Laterally Supported)."
            },
            {
                id: 35,
                category: "Steel Design",
                question: "Plastic Moment Mp is calculated as:",
                options: ["Fy Sx", "Fy Zx", "Fu Zx", "E I"],
                correct: 1,
                solution: "FORMULA: Mp = Fy * Zx (Plastic Modulus). \nMy = Fy * Sx (Elastic Modulus)."
            },
            {
                id: 36,
                category: "Steel Design",
                question: "Value of resistance factor φb for steel beams in flexure (LRFD)?",
                options: ["0.75", "0.85", "0.90", "1.67"],
                correct: 2,
                solution: "CODE: φ = 0.90 for Flexure (Yielding/Buckling)."
            },
            {
                id: 37,
                category: "Steel Design",
                question: "Slenderness ratio of a compression member is defined as:",
                options: ["KL/r", "L/r", "KL", "r/L"],
                correct: 0,
                solution: "DEFINITION: Slenderness λ = KL / r. (Effective Length / Radius of Gyration)."
            },
            {
                id: 38,
                category: "Steel Design",
                question: "If slenderness KL/r increases, critical buckling stress Fcr:",
                options: ["Increases", "Decreases", "Constant", "Becomes zero"],
                correct: 1,
                solution: "CONCEPT: The more slender (skinny/long) the column, the easier it is to buckle, so its strength (Fcr) Decreases."
            },
            {
                id: 39,
                category: "Steel Design",
                question: "Euler Buckling Load formula is applicable for:",
                options: ["Short columns", "Intermediate columns", "Long/Elastic columns", "Concrete only"],
                correct: 2,
                solution: "THEORY: Euler formula assumes purely elastic behavior, which only happens in Long/Slender columns."
            },
            {
                id: 40,
                category: "Steel Design",
                question: "Interaction equation for Axial + Moment generally checks that sum of ratios is:",
                options: ["> 1.0", "≤ 1.0", "= 0", "≤ 0.5"],
                correct: 1,
                solution: "FORMULA: (Pr/Pc) + (Mr/Mc) ≤ 1.0. This is the 'Unity Check'."
            },
            {
                id: 41,
                category: "Steel Design",
                question: "In bolted connections, 'bearing failure' refers to:",
                options: ["Bolt shearing", "Plate tearing at edge", "Crushing of plate behind bolt", "Bolt stretching"],
                correct: 2,
                solution: "DEFINITION: Bearing failure is when the bolt acts like a knife and crushes/deforms the steel plate material contacting it."
            },
            {
                id: 42,
                category: "Steel Design",
                question: "Standard round hole diameter is usually taken as bolt diameter plus:",
                options: ["1 mm", "1.6 mm or 2mm", "3 mm", "0 mm"],
                correct: 1,
                solution: "PRACTICE: Standard holes are d + 1.6mm (1/16 inch) oversize to allow fit-up."
            },
            {
                id: 43,
                category: "Steel Design",
                question: "Effective throat thickness of a fillet weld is typically:",
                options: ["0.707 × size", "0.50 × size", "Equal to size", "Plate thickness"],
                correct: 0,
                solution: "FORMULA: Throat = size × cos(45°) = 0.707 × size."
            },
            {
                id: 44,
                category: "Steel Design",
                question: "Most common electrode for SMAW structural steel?",
                options: ["E60XX", "E70XX", "E80XX", "E100XX"],
                correct: 1,
                solution: "PRACTICE: E70XX (70 ksi or ~485 MPa) matches standard A36 or Gr 50 steel strength well."
            },
            {
                id: 45,
                category: "Steel Design",
                question: "Modulus of Elasticity E for structural steel is approx:",
                options: ["200 GPa", "250 GPa", "150 GPa", "4700 MPa"],
                correct: 0,
                solution: "CONSTANT: E_steel ≈ 200,000 MPa = 200 GPa. (Concrete is roughly 25 GPa)."
            },
            // --- MATERIALS ---
            {
                id: 46,
                category: "Materials & Testing",
                question: "Standard size of concrete cylinder for compressive strength:",
                options: ["100x200 mm", "150x300 mm", "150x150 mm", "100x100 mm"],
                correct: 1,
                solution: "STANDARD: The standard cylinder is 6 inches x 12 inches, or 150 mm x 300 mm."
            },
            {
                id: 47,
                category: "Materials & Testing",
                question: "Slump Test is primarily used to measure:",
                options: ["Strength", "Durability", "Consistency/Workability", "Air Content"],
                correct: 2,
                solution: "CONCEPT: Slump measures how 'wet' or flowable the mix is (consistency)."
            },
            {
                id: 48,
                category: "Materials & Testing",
                question: "Approximate weight of reinforced concrete per cubic meter?",
                options: ["1800 kg/m³", "2000 kg/m³", "2400 kg/m³", "7850 kg/m³"],
                correct: 2,
                solution: "CONSTANT: Normal concrete is ~23.5 kN/m³ or ~2400 kg/m³. Steel is 7850 kg/m³."
            },
            {
                id: 49,
                category: "Materials & Testing",
                question: "Liquid Limit (LL) minus Plastic Limit (PL) is:",
                options: ["Liquidity Index", "Plasticity Index", "Shrinkage Limit", "Consistency Index"],
                correct: 1,
                solution: "FORMULA: PI = LL - PL. It defines the range of water content where soil behaves plastically."
            },
            {
                id: 50,
                category: "Materials & Testing",
                question: "Test used to determine hardness of bitumen/asphalt?",
                options: ["Ductility", "Penetration", "Flash Point", "Viscosity"],
                correct: 1,
                solution: "TESTING: The Penetration Test measures how deep a standard needle sinks into the asphalt. Deeper = Softer."
            }
        ];

        // --- MAIN APPLICATION COMPONENT ---
        function App() {
            // State
            const [appState, setAppState] = useState('menu'); // 'menu', 'quiz', 'result'
            const [selectedCategories, setSelectedCategories] = useState([]);
            const [targetQuestionCount, setTargetQuestionCount] = useState(10);
            const [activeQuestions, setActiveQuestions] = useState([]);
            const [currentQuestionIndex, setCurrentQuestionIndex] = useState(0);
            const [score, setScore] = useState(0);
            const [selectedOption, setSelectedOption] = useState(null);
            const [isCorrect, setIsCorrect] = useState(null);
            const [showSolution, setShowSolution] = useState(false);

            // Derived Data
            const allCategories = useMemo(() => [...new Set(allQuestions.map(q => q.category))], []);

            // Initialize categories on mount
            useEffect(() => {
                setSelectedCategories(allCategories);
            }, [allCategories]);

            // Handlers
            const toggleCategory = (cat) => {
                if (selectedCategories.includes(cat)) {
                    if (selectedCategories.length > 1) { 
                        setSelectedCategories(selectedCategories.filter(c => c !== cat));
                    }
                } else {
                    setSelectedCategories([...selectedCategories, cat]);
                }
            };

            const selectAllCategories = () => setSelectedCategories(allCategories);

            const startQuiz = () => {
                let filtered = allQuestions.filter(q => selectedCategories.includes(q.category));
                
                // Fisher-Yates Shuffle
                for (let i = filtered.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [filtered[i], filtered[j]] = [filtered[j], filtered[i]];
                }

                const finalQuestions = filtered.slice(0, Math.min(targetQuestionCount, filtered.length));

                if (finalQuestions.length === 0) return;

                setActiveQuestions(finalQuestions);
                setCurrentQuestionIndex(0);
                setScore(0);
                setSelectedOption(null);
                setIsCorrect(null);
                setShowSolution(false);
                setAppState('quiz');
            };

            const handleAnswerOptionClick = (index) => {
                if (selectedOption !== null) return;
                
                setSelectedOption(index);
                const correct = index === activeQuestions[currentQuestionIndex].correct;
                setIsCorrect(correct);
                if (correct) setScore(score + 1);
                setShowSolution(true);
            };

            const handleNextQuestion = () => {
                const nextQ = currentQuestionIndex + 1;
                if (nextQ < activeQuestions.length) {
                    setCurrentQuestionIndex(nextQ);
                    setSelectedOption(null);
                    setIsCorrect(null);
                    setShowSolution(false);
                } else {
                    setAppState('result');
                }
            };

            const returnToMenu = () => {
                setAppState('menu');
                setSelectedOption(null);
                setIsCorrect(null);
                setShowSolution(false);
            };

            // --- MENU RENDER ---
            if (appState === 'menu') {
                const availableCount = allQuestions.filter(q => selectedCategories.includes(q.category)).length;
                
                return (
                    <div className="min-h-screen p-4 md:p-8 flex items-center justify-center">
                        <div className="w-full max-w-2xl bg-white rounded-2xl shadow-xl border border-slate-200 overflow-hidden">
                            <div className="bg-blue-600 p-8 text-white">
                                <div className="flex items-center gap-3 mb-2">
                                    <Icons.BookOpen size={32} />
                                    <h1 className="text-3xl font-bold">Review Setup</h1>
                                </div>
                                <p className="text-blue-100 opacity-90">Customize your practice session.</p>
                            </div>

                            <div className="p-8">
                                <div className="mb-8">
                                    <div className="flex items-center justify-between mb-4">
                                        <div className="flex items-center gap-2 text-slate-700 font-bold">
                                            <Icons.Layers size={20} />
                                            <span>Select Topics</span>
                                        </div>
                                        <button onClick={selectAllCategories} className="text-xs text-blue-600 font-bold hover:underline">
                                            Select All
                                        </button>
                                    </div>
                                    
                                    <div className="grid grid-cols-1 md:grid-cols-2 gap-3">
                                        {allCategories.map(cat => {
                                            const isSelected = selectedCategories.includes(cat);
                                            return (
                                                <button
                                                    key={cat}
                                                    onClick={() => toggleCategory(cat)}
                                                    className={`p-3 rounded-lg text-sm font-medium text-left transition-all flex items-center gap-3 border ${
                                                        isSelected 
                                                        ? 'bg-blue-50 border-blue-500 text-blue-700' 
                                                        : 'bg-white border-slate-200 text-slate-400 hover:border-slate-300'
                                                    }`}
                                                >
                                                    <div className={`w-5 h-5 rounded-full border flex items-center justify-center ${
                                                        isSelected ? 'bg-blue-600 border-blue-600' : 'border-slate-300'
                                                    }`}>
                                                        {isSelected && <Icons.CheckCircle size={12} className="text-white" />}
                                                    </div>
                                                    {cat}
                                                </button>
                                            );
                                        })}
                                    </div>
                                </div>

                                <div className="mb-10">
                                    <div className="flex items-center justify-between mb-4">
                                        <div className="flex items-center gap-2 text-slate-700 font-bold">
                                            <Icons.ListFilter size={20} />
                                            <span>Number of Items</span>
                                        </div>
                                        <span className="text-2xl font-black text-blue-600">{targetQuestionCount}</span>
                                    </div>
                                    <input 
                                        type="range" 
                                        min="5" 
                                        max={50} 
                                        step="5"
                                        value={targetQuestionCount}
                                        onChange={(e) => setTargetQuestionCount(Number(e.target.value))}
                                        className="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-blue-600"
                                    />
                                    <div className="flex justify-between text-xs text-slate-400 mt-2 font-medium">
                                        <span>Quick (5)</span>
                                        <span>Standard (20)</span>
                                        <span>Full Mock (50)</span>
                                    </div>
                                </div>

                                <div className="space-y-3">
                                    <button 
                                        onClick={startQuiz}
                                        disabled={availableCount === 0}
                                        className="w-full bg-blue-600 hover:bg-blue-700 text-white text-lg font-bold py-4 px-6 rounded-xl flex items-center justify-center gap-2 transition-all shadow-lg hover:shadow-blue-200 disabled:opacity-50 disabled:cursor-not-allowed"
                                    >
                                        <Icons.PlayCircle size={24} />
                                        Start Review Session
                                    </button>
                                    
                                    {availableCount < targetQuestionCount && availableCount > 0 && (
                                        <p className="text-center text-xs text-orange-500 font-medium">
                                            Note: Only {availableCount} questions available for selected topics.
                                        </p>
                                    )}
                                </div>
                            </div>
                        </div>
                    </div>
                );
            }

            // --- QUIZ & RESULT RENDER ---
            return (
                <div className="min-h-screen p-4 md:p-8">
                    <div className="max-w-2xl mx-auto">
                        <header className="mb-6 flex items-center justify-between bg-white p-4 rounded-xl shadow-sm border border-slate-200">
                            <div className="flex items-center gap-3">
                                <button onClick={returnToMenu} className="p-2 hover:bg-slate-100 rounded-lg text-slate-400 transition-colors">
                                    <Icons.Settings size={20} />
                                </button>
                                <div>
                                    <h1 className="font-bold text-lg leading-tight">CivEng Review</h1>
                                    <p className="text-xs text-slate-500 font-medium">
                                        {appState === 'quiz' ? activeQuestions[currentQuestionIndex].category : 'Session Complete'}
                                    </p>
                                </div>
                            </div>
                            {appState === 'quiz' && (
                                <div className="text-right">
                                    <div className="text-sm font-semibold text-blue-600">
                                        {currentQuestionIndex + 1} / {activeQuestions.length}
                                    </div>
                                    <div className="text-xs text-slate-400">Question</div>
                                </div>
                            )}
                        </header>

                        {appState === 'result' ? (
                            <div className="bg-white rounded-2xl shadow-lg border border-slate-200 p-8 text-center animate-in fade-in zoom-in duration-300">
                                <div className="mx-auto w-24 h-24 bg-blue-50 rounded-full flex items-center justify-center mb-6">
                                    <Icons.Award size={48} className="text-blue-600" />
                                </div>
                                <h2 className="text-2xl font-bold mb-2">Session Complete!</h2>
                                <p className="text-slate-500 mb-6">Here is how you performed.</p>
                                
                                <div className="text-5xl font-black text-slate-800 mb-2">
                                    {score} <span className="text-2xl text-slate-400 font-normal">/ {activeQuestions.length}</span>
                                </div>
                                
                                <div className={`inline-block px-4 py-1 rounded-full text-sm font-bold mb-8 ${
                                    (score / activeQuestions.length) >= 0.65 ? 'bg-green-100 text-green-700' : 'bg-orange-100 text-orange-700'
                                }`}>
                                    {((score / activeQuestions.length) * 100).toFixed(0)}% - {(score / activeQuestions.length) >= 0.65 ? 'PASSED' : 'Needs Review'}
                                </div>

                                <button 
                                    onClick={returnToMenu}
                                    className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-xl flex items-center justify-center gap-2 transition-all"
                                >
                                    <Icons.RotateCcw size={20} />
                                    Setup New Session
                                </button>
                            </div>
                        ) : (
                            <>
                                <div className="bg-white rounded-2xl shadow-md border border-slate-200 overflow-hidden mb-4">
                                    <div className="p-6 md:p-8">
                                        <h3 className="text-lg md:text-xl font-medium mb-6 leading-relaxed">
                                            {activeQuestions[currentQuestionIndex].question}
                                        </h3>

                                        <div className="space-y-3">
                                            {activeQuestions[currentQuestionIndex].options.map((option, index) => {
                                                let btnClass = "w-full text-left p-4 rounded-xl border-2 transition-all duration-200 font-medium relative ";
                                                
                                                if (selectedOption === null) {
                                                    btnClass += "border-slate-100 hover:border-blue-200 hover:bg-blue-50";
                                                } else {
                                                    if (index === activeQuestions[currentQuestionIndex].correct) {
                                                        btnClass += "border-green-500 bg-green-50 text-green-800";
                                                    } else if (index === selectedOption) {
                                                        btnClass += "border-red-500 bg-red-50 text-red-800";
                                                    } else {
                                                        btnClass += "border-slate-100 opacity-50";
                                                    }
                                                }

                                                return (
                                                    <button
                                                        key={index}
                                                        onClick={() => handleAnswerOptionClick(index)}
                                                        disabled={selectedOption !== null}
                                                        className={btnClass}
                                                    >
                                                        <div className="flex items-center justify-between">
                                                            <span>{option}</span>
                                                            {selectedOption !== null && index === activeQuestions[currentQuestionIndex].correct && (
                                                                <Icons.CheckCircle size={20} className="text-green-600" />
                                                            )}
                                                            {selectedOption === index && index !== activeQuestions[currentQuestionIndex].correct && (
                                                                <Icons.XCircle size={20} className="text-red-500" />
                                                            )}
                                                        </div>
                                                    </button>
                                                );
                                            })}
                                        </div>
                                    </div>
                                </div>

                                {showSolution && (
                                    <div className="animate-in slide-in-from-bottom-4 duration-300">
                                        <div className="bg-blue-900 text-white rounded-xl p-6 shadow-lg mb-20 md:mb-6 relative overflow-hidden">
                                            <div className="absolute top-0 right-0 p-4 opacity-10">
                                                <Icons.Lightbulb size={100} />
                                            </div>
                                            <div className="relative z-10">
                                                <div className="flex items-center gap-2 mb-2 text-blue-200 font-bold uppercase text-xs tracking-widest">
                                                    <Icons.Lightbulb size={14} />
                                                    Strategic Solution
                                                </div>
                                                <p className="text-blue-50 leading-relaxed font-medium whitespace-pre-line">
                                                    {activeQuestions[currentQuestionIndex].solution}
                                                </p>
                                            </div>
                                        </div>

                                        <div className="fixed bottom-0 left-0 w-full p-4 bg-white border-t border-slate-200 md:relative md:bg-transparent md:border-0 md:p-0 z-50">
                                            <button 
                                                onClick={handleNextQuestion}
                                                className="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-4 px-6 rounded-xl flex items-center justify-center gap-2 transition-all shadow-lg md:shadow-none"
                                            >
                                                {currentQuestionIndex === activeQuestions.length - 1 ? 'Finish Session' : 'Next Question'}
                                                <Icons.ChevronRight size={20} />
                                            </button>
                                        </div>
                                    </div>
                                )}
                            </>
                        )}
                    </div>
                </div>
            );
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
