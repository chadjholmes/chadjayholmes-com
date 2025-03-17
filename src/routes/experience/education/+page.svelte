<script lang="ts">
  import { fade } from 'svelte/transition';
  
  interface Course {
    code: string;
    title: string;
    credits: string;
    grade: string;
  }

  const degreeInfo = {
    university: "Brigham Young University",
    degree: "BS Computer Science",
    graduationDate: "June 2023",
    honors: "Cum Laude",
    cumulativeGpa: "3.94",
    majorGpa: "3.92"
  };

  // Add a state variable for CS course filtering
  let showOnlyCs = false;

  // Helper function to check if a course is a CS course
  const isCsCourse = (code: string) => code.startsWith('C S');

  const advancedPlacement = [
    { course: "ENGLISH LANGUAGE & COMPOSITION", score: "4", credit: "WRTG 150 - Writing & Rhetoric (3.0 credits)" },
    { course: "HUMAN GEOGRAPHY", score: "4", credit: "GEOG 130 - Intro to Human Geography (3.0 credits)" },
    { course: "MATH - CALCULUS AB", score: "4", credit: "MATH 112 - Calculus 1 (4.0 credits), MATH 110 - College Algebra (3.0 credits)" }
  ];
  
  // Organize courses by term with index signature
  const coursesByTerm: { [term: string]: Course[] } = {
    "Spring 2019": [
      { code: "MATH 112", title: "Calculus 1", credits: "4.00", grade: "A" },
      { code: "REL A 211", title: "The New Testament", credits: "2.00", grade: "A" }
    ],
    "Fall 2019": [
      { code: "A HTG 100", title: "American Heritage", credits: "3.00", grade: "A" },
      { code: "MATH 113", title: "Calculus 2", credits: "4.00", grade: "A" },
      { code: "ME EN 191", title: "New Student Seminar", credits: "0.50", grade: "P" },
      { code: "PHSCS 121", title: "Intro to Newtonian Mechanics", credits: "3.00", grade: "A" },
      { code: "REL C 200", title: "The Eternal Family", credits: "2.00", grade: "A" }
    ],
    "Winter 2020": [
      { code: "CHEM 105", title: "Gen College Chem 1+Lab Integr", credits: "4.00", grade: "A-" },
      { code: "MATH 213", title: "Elementary Linear Algebra", credits: "2.00", grade: "A" },
      { code: "ME EN 101", title: "Static Systems in Me En", credits: "3.00", grade: "A-" },
      { code: "MUSIC 101", title: "Introduction to Music", credits: "3.00", grade: "A" },
      { code: "REL A 121", title: "The Book of Mormon", credits: "2.00", grade: "A" }
    ],
    "Fall 2020": [
      { code: "C S 142", title: "Intro to Computer Programming", credits: "3.00", grade: "A" },
      { code: "MATH 215", title: "Computational Linear Algebra", credits: "1.00", grade: "A" },
      { code: "REL A 122", title: "The Book of Mormon", credits: "2.00", grade: "A" },
      { code: "SOC 111", title: "Introductory Sociology", credits: "3.00", grade: "A" },
      { code: "STAT 121", title: "Principles of Statistics", credits: "3.00", grade: "A" }
    ],
    "Winter 2021": [
      { code: "C S 202", title: "Software Engineering Lab 1", credits: "1.00", grade: "A" },
      { code: "C S 235", title: "Data Structures", credits: "3.00", grade: "A" },
      { code: "IHUM 201", title: "Western Humanities 1", credits: "3.00", grade: "A" },
      { code: "NDFS 100", title: "Essentials of Human Nutrition", credits: "3.00", grade: "A" },
      { code: "REL C 225", title: "Foundations of the Restoration", credits: "2.00", grade: "A" }
    ],
    "Fall 2021": [
      { code: "EXDM 223R", title: "Mountain Biking", credits: "2.00", grade: "P" },
      { code: "C S 203", title: "Software Engineering Lab 2", credits: "1.00", grade: "A" },
      { code: "C S 224", title: "Computer Systems", credits: "3.00", grade: "A" },
      { code: "C S 236", title: "Discrete Structure", credits: "3.00", grade: "A" },
      { code: "C S 260", title: "Web Programming", credits: "3.00", grade: "A" },
      { code: "REL A 275", title: "Tchgs & Doctrine of B of M", credits: "2.00", grade: "A" }
    ],
    "Winter 2022": [
      { code: "C S 240", title: "Adv Programming Concepts", credits: "4.00", grade: "A" },
      { code: "REL A 250", title: "Christ & the Everlstng Gospel", credits: "2.00", grade: "A" },
      { code: "SFL 260", title: "Family Finance", credits: "3.00", grade: "A" },
      { code: "WRTG 316", title: "Technical Communication", credits: "3.00", grade: "A" }
    ],
    "Fall 2022": [
      { code: "C S 204", title: "Software Engineering Lab 3", credits: "1.00", grade: "A" },
      { code: "C S 312", title: "Algorithm Design & Analysis", credits: "3.00", grade: "A" },
      { code: "C S 356", title: "Designing the User Experience", credits: "3.00", grade: "A" },
      { code: "C S 405", title: "Software Business", credits: "3.00", grade: "A" },
      { code: "C S 498R", title: "Undergraduate Special Projects", credits: "3.00", grade: "A" },
      { code: "MUSIC 202", title: "Civilization: Music 2", credits: "3.00", grade: "A" }
    ],
    "Winter 2023": [
      { code: "C S 324", title: "Systems Programming", credits: "3.00", grade: "A-" },
      { code: "C S 393", title: "Adv Algorithms & Probl Solving", credits: "3.00", grade: "A" },
      { code: "C S 452", title: "Database Modeling Concepts", credits: "3.00", grade: "A" },
      { code: "C S 498R", title: "Undergraduate Special Projects", credits: "3.00", grade: "A" },
      { code: "IT&C 515R", title: "Special Topics in IT&C", credits: "3.00", grade: "A" }
    ],
    "Spring 2023": [
      { code: "C S 453", title: "Fund of Information Retrieval", credits: "3.00", grade: "B" },
      { code: "PHIL 110", title: "Introduction to Philosophy", credits: "3.00", grade: "A" }
    ]
  };

  // Get all terms in chronological order
  const termOrder = [
    "Spring 2019",
    "Fall 2019",
    "Winter 2020",
    "Fall 2020",
    "Winter 2021",
    "Fall 2021",
    "Winter 2022",
    "Fall 2022",
    "Winter 2023",
    "Spring 2023"
  ];
</script>

<div class="education-page">
  <div class="content">
    <header>
      <h1>Academic Transcript</h1>
      <div class="degree-info">
        <h2>{degreeInfo.university}</h2>
        <div class="degree-details">
          <p><strong>Degree:</strong> {degreeInfo.degree}</p>
          <p><strong>Graduation:</strong> {degreeInfo.graduationDate}</p>
          <p><strong>Honors:</strong> {degreeInfo.honors}</p>
          <p><strong>Cumulative GPA:</strong> {degreeInfo.cumulativeGpa}</p>
          <p><strong>Major GPA:</strong> {degreeInfo.majorGpa}</p>
        </div>
      </div>
    </header>

    <section class="transcript">
      <div class="filter-controls">
        <button 
          class="filter-button" 
          class:active={showOnlyCs}
          on:click={() => showOnlyCs = !showOnlyCs}
        >
          {showOnlyCs ? 'Show All Courses' : 'Show Only CS Courses'}
        </button>
      </div>
      
      <div class="course-list">
        {#each termOrder as term}
          <!-- Only render the term section if it has courses after filtering -->
          {@const termCourses = showOnlyCs 
            ? coursesByTerm[term].filter(course => isCsCourse(course.code))
            : coursesByTerm[term]}
          
          {#if termCourses.length > 0}
            <div class="term-section" transition:fade={{ duration: 150 }}>
              <h3 class="term-heading">{term}</h3>
              <table class="courses-table">
                <thead>
                  <tr>
                    <th>Course</th>
                    <th>Title</th>
                    <th>Credits</th>
                    <th>Grade</th>
                  </tr>
                </thead>
                <tbody>
                  {#each termCourses as course}
                    <tr class:cs-course={isCsCourse(course.code)}>
                      <td>{course.code}</td>
                      <td class="course-title">{course.title}</td>
                      <td>{course.credits}</td>
                      <td class="grade">{course.grade}</td>
                    </tr>
                  {/each}
                </tbody>
              </table>
            </div>
          {/if}
        {/each}
      </div>
    </section>

    <section class="additional-info">
      <div class="section-container">
        <h3>Advanced Placement</h3>
        <table class="ap-table">
          <thead>
            <tr>
              <th>Exam</th>
              <th>Score</th>
              <th>Credits Awarded</th>
            </tr>
          </thead>
          <tbody>
            {#each advancedPlacement as ap}
              <tr>
                <td>{ap.course}</td>
                <td>{ap.score}</td>
                <td>{ap.credit}</td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </section>

    <div class="actions">
      <a href="/experience" class="back-button">← Back to Experience</a>
    </div>
  </div>
</div>

<style>
  .education-page {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem 1rem;
    color: var(--text-color);
  }

  .content {
    background-color: rgba(10, 10, 20, 0.8);
    border-radius: 0.5rem;
    padding: 2rem;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  }

  header {
    margin-bottom: 2rem;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    padding-bottom: 1rem;
  }

  h1 {
    font-size: 2.5rem;
    margin-bottom: 1rem;
    color: var(--text-accent);
  }

  h2 {
    font-size: 1.8rem;
    margin-bottom: 0.5rem;
    color: var(--text-primary);
  }

  h3 {
    font-size: 1.4rem;
    margin: 1.5rem 0 0.5rem;
    color: var(--text-accent);
  }

  .degree-details {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 0.5rem;
    margin-top: 0.5rem;
  }

  .degree-details p {
    margin: 0.25rem 0;
  }

  .term-section {
    margin-bottom: 2rem;
  }

  .term-heading {
    background-color: rgba(93, 63, 211, 0.2);
    padding: 0.5rem 1rem;
    border-radius: 0.25rem;
    margin-bottom: 0.5rem;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 1.5rem;
    font-size: 0.95rem;
  }

  th {
    text-align: left;
    padding: 0.5rem;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    color: var(--text-accent);
    font-weight: 600;
  }

  td {
    padding: 0.5rem;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }

  .course-title {
    width: 50%;
  }

  .grade {
    font-weight: 600;
  }

  .section-container {
    margin-bottom: 2rem;
    background-color: rgba(15, 15, 30, 0.5);
    padding: 1rem;
    border-radius: 0.25rem;
  }

  .actions {
    margin-top: 2rem;
    display: flex;
    justify-content: flex-start;
  }

  .back-button {
    display: inline-block;
    padding: 0.5rem 1.25rem;
    background-color: rgba(93, 63, 211, 0.2);
    color: var(--text-accent);
    border-radius: 0.25rem;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s ease;
  }

  .back-button:hover {
    background-color: rgba(93, 63, 211, 0.4);
  }

  @media (max-width: 768px) {
    .content {
      padding: 1.5rem 1rem;
    }

    h1 {
      font-size: 2rem;
    }

    h2 {
      font-size: 1.5rem;
    }

    .degree-details {
      grid-template-columns: 1fr;
    }

    table {
      font-size: 0.85rem;
    }

    th, td {
      padding: 0.4rem;
    }
  }

  .filter-controls {
    margin-bottom: 1.5rem;
    display: flex;
    justify-content: flex-end;
  }

  .filter-button {
    background: rgba(125, 211, 252, 0.1);
    border: 1px solid var(--text-accent);
    color: var(--text-accent);
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 0.9rem;
  }

  .filter-button:hover {
    background: rgba(125, 211, 252, 0.2);
  }

  .filter-button.active {
    background: rgba(125, 211, 252, 0.3);
    color: var(--text-primary);
  }

  .cs-course {
    background-color: rgba(93, 63, 211, 0.08);
  }
</style> 