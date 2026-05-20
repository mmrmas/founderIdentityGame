import './styles.css';

const identityPillars = [
  {
    title: 'Cross-disciplinary Builder',
    short: 'Science, AI, and implementation belong in the same conversation.',
    prompt: 'Show how your range helps you connect research, technology, and real-world execution.',
    anchors: ['Science', 'AI', 'Implementation'],
  },
  {
    title: 'Practical Innovation',
    short: 'Ideas matter most when they become usable systems.',
    prompt: 'Explain how you move from possibility to something people can actually use.',
    anchors: ['Prototype', 'Workflow', 'Adoption'],
  },
  {
    title: 'Societal Impact',
    short: 'Technology should create value beyond technical novelty.',
    prompt: 'Make the real-world relevance concrete, human, and believable.',
    anchors: ['Relevance', 'Access', 'Outcomes'],
  },
  {
    title: 'Empowerment',
    short: 'Strong systems create opportunities for other people.',
    prompt: 'Frame your work as a way to unlock agency, capability, or participation.',
    anchors: ['Opportunity', 'Confidence', 'Capability'],
  },
  {
    title: 'Curiosity & Adaptability',
    short: 'Continuous learning is part of the operating system.',
    prompt: 'Turn your self-directed path into evidence of resilience and growth.',
    anchors: ['Learning', 'Iteration', 'Range'],
  },
];

const challengeCards = [
  'Explain your mission',
  'Explain your leadership style to a future employee',
  'Explain your value to a biotech partner',
  'Explain why you are different from a pure researcher',
  'Explain your vision without sounding unrealistic',
  'Explain your biggest leadership risk',
  'Explain why your self-taught path is a strength',
  'Explain what societal impact means concretely',
  'Explain what makes your founder story memorable',
  'Explain why now is the right moment',
];

const audiences = [
  { title: 'Investor', cares: 'scalability, timing, and narrative conviction' },
  { title: 'Scientist', cares: 'credibility, rigor, and intellectual honesty' },
  { title: 'Employee', cares: 'leadership, culture, and trust' },
  { title: 'Corporate Partner', cares: 'execution, reliability, and fit' },
  { title: 'NGO / Global Health Org', cares: 'societal value, access, and grounded impact' },
  { title: 'Technical Cofounder', cares: 'depth, judgment, and product direction' },
  { title: 'Customer', cares: 'usefulness, clarity, and outcomes' },
];

const constraints = [
  'Explain without using the word AI',
  'Explain in one sentence',
  'Use a real-world example',
  'Speak as if you are on stage',
  'Speak casually over coffee',
  'Use no technical jargon',
  'Start with a concrete problem',
  'End with why it matters',
];

const antiBsClaims = [
  'I combine innovation with practical execution.',
  'I build across science, technology, and business.',
  'I care about societal impact.',
  'I empower people through better systems.',
  'My self-taught path is a strength.',
  'I can translate complex ideas into useful products.',
  'I adapt quickly when the context changes.',
  'I lead with curiosity and clarity.',
];

const state = {
  mode: 'classic',
  duration: 60,
  remaining: 60,
  timerId: null,
  isRunning: false,
  includeConstraint: true,
  current: null,
  history: [],
  locks: {
    pillar: false,
    challenge: false,
    audience: false,
    constraint: false,
  },
  scores: {
    clarity: 0,
    coherence: 0,
    memorability: 0,
  },
};

const app = document.querySelector('#app');

function sample(list) {
  return list[Math.floor(Math.random() * list.length)];
}

function createRound() {
  const previous = state.current || {};
  state.current = {
    pillar: state.locks.pillar && previous.pillar ? previous.pillar : sample(identityPillars),
    challenge: state.locks.challenge && previous.challenge ? previous.challenge : sample(challengeCards),
    audience: state.locks.audience && previous.audience ? previous.audience : sample(audiences),
    constraint:
      state.locks.constraint && previous.constraint ? previous.constraint : sample(constraints),
    antiBsClaim: sample(antiBsClaims),
  };
  resetTimer();
  render();
}

function resetTimer() {
  window.clearInterval(state.timerId);
  state.timerId = null;
  state.isRunning = false;
  state.remaining = state.duration;
}

function startTimer() {
  if (state.isRunning) return;

  state.isRunning = true;
  state.timerId = window.setInterval(() => {
    state.remaining -= 1;

    if (state.remaining <= 0) {
      state.remaining = 0;
      window.clearInterval(state.timerId);
      state.timerId = null;
      state.isRunning = false;
    }

    renderTimer();
  }, 1000);

  renderTimer();
}

function pauseTimer() {
  window.clearInterval(state.timerId);
  state.timerId = null;
  state.isRunning = false;
  renderTimer();
}

function completeRound() {
  if (!state.current) return;

  const entry = {
    ...state.current,
    mode: state.mode,
    scores: { ...state.scores },
    createdAt: new Date(),
  };

  state.history = [entry, ...state.history].slice(0, 6);
  state.scores = { clarity: 0, coherence: 0, memorability: 0 };
  createRound();
}

function toggleLock(key) {
  state.locks[key] = !state.locks[key];
  render();
}

function setScore(key, value) {
  state.scores[key] = Number(value);
  renderScoreSummary();
}

function setMode(mode) {
  state.mode = mode;
  resetTimer();
  render();
}

function setDuration(duration) {
  state.duration = Number(duration);
  resetTimer();
  render();
}

function cardTemplate(type, title, body, meta, locked) {
  return `
    <article class="game-card ${type}">
      <div class="card-topline">
        <span>${type}</span>
        <button class="icon-button ${locked ? 'active' : ''}" data-lock="${type}" aria-label="${locked ? 'Unlock' : 'Lock'} ${type}">
          ${locked ? 'Locked' : 'Lock'}
        </button>
      </div>
      <h2>${title}</h2>
      <p>${body}</p>
      ${meta ? `<div class="card-meta">${meta}</div>` : ''}
    </article>
  `;
}

function renderClassicRound() {
  const { pillar, challenge, audience, constraint } = state.current;
  const constraintCard = state.includeConstraint
    ? cardTemplate('constraint', constraint, 'Use this constraint to force deeper mastery.', '', state.locks.constraint)
    : '';

  return `
    <section class="card-grid" aria-label="Current practice cards">
      ${cardTemplate(
        'pillar',
        pillar.title,
        pillar.short,
        `<span>${pillar.anchors.join('</span><span>')}</span>`,
        state.locks.pillar,
      )}
      ${cardTemplate('challenge', challenge, 'Answer this naturally from the selected identity pillar.', '', state.locks.challenge)}
      ${cardTemplate('audience', audience.title, `They care about ${audience.cares}.`, '', state.locks.audience)}
      ${constraintCard}
    </section>

    <section class="practice-panel">
      <div>
        <p class="section-label">Speaking brief</p>
        <h2>${challenge} for a ${audience.title.toLowerCase()}</h2>
        <p>${pillar.prompt}</p>
      </div>
      <div class="answer-frame">
        <span>Opening move</span>
        <p>Start with a concrete situation, connect it to <strong>${pillar.title}</strong>, then make the value obvious for this audience.</p>
      </div>
    </section>
  `;
}

function renderAntiBsRound() {
  const { antiBsClaim, audience } = state.current;

  return `
    <section class="anti-bs-layout">
      <article class="game-card anti-bs">
        <div class="card-topline">
          <span>claim</span>
        </div>
        <h2>${antiBsClaim}</h2>
        <p>Can you give a concrete example from your real life immediately?</p>
        <div class="card-meta"><span>Project</span><span>Workshop</span><span>Prototype</span><span>Team moment</span></div>
      </article>
      <article class="game-card audience">
        <div class="card-topline">
          <span>listener</span>
          <button class="icon-button ${state.locks.audience ? 'active' : ''}" data-lock="audience" aria-label="${state.locks.audience ? 'Unlock' : 'Lock'} audience">
            ${state.locks.audience ? 'Locked' : 'Lock'}
          </button>
        </div>
        <h2>${audience.title}</h2>
        <p>Make the proof meaningful for someone who cares about ${audience.cares}.</p>
      </article>
    </section>

    <section class="practice-panel">
      <div>
        <p class="section-label">Anti-BS drill</p>
        <h2>Replace abstraction with evidence</h2>
        <p>Name the exact example, what you did, what changed, and what it proves about you as a founder.</p>
      </div>
      <div class="answer-frame">
        <span>Proof structure</span>
        <p>When this happened, I did this, it created this result, and that is why the claim is real.</p>
      </div>
    </section>
  `;
}

function renderTimer() {
  const timer = document.querySelector('[data-timer]');
  if (!timer) return;

  const progress = state.remaining / state.duration;
  const degrees = Math.max(0, Math.round(progress * 360));
  timer.style.setProperty('--timer-deg', `${degrees}deg`);
  timer.querySelector('strong').textContent = state.remaining;
  timer.querySelector('span').textContent = state.isRunning ? 'speaking' : 'ready';

  const startButton = document.querySelector('[data-action="start"]');
  if (startButton) startButton.textContent = state.isRunning ? 'Running' : 'Start';
}

function renderScoreSummary() {
  const summary = document.querySelector('[data-score-summary]');
  if (!summary) return;

  const values = Object.values(state.scores);
  const total = values.reduce((sum, value) => sum + value, 0);
  summary.textContent = total === 0 ? 'No score yet' : `${total}/15 practice score`;
}

function renderHistory() {
  if (state.history.length === 0) {
    return '<p class="empty-state">Completed rounds will appear here.</p>';
  }

  return state.history
    .map((entry) => {
      const title = entry.mode === 'anti-bs' ? entry.antiBsClaim : entry.challenge;
      const context =
        entry.mode === 'anti-bs'
          ? `Anti-BS proof for ${entry.audience.title}`
          : `${entry.pillar.title} for ${entry.audience.title}`;
      const score = Object.values(entry.scores).reduce((sum, value) => sum + value, 0);

      return `
        <li>
          <strong>${title}</strong>
          <span>${context}</span>
          <small>${score}/15</small>
        </li>
      `;
    })
    .join('');
}

function render() {
  if (!state.current) {
    state.current = {
      pillar: sample(identityPillars),
      challenge: sample(challengeCards),
      audience: sample(audiences),
      constraint: sample(constraints),
      antiBsClaim: sample(antiBsClaims),
    };
  }

  app.innerHTML = `
    <main class="shell">
      <header class="app-header">
        <div>
          <p class="eyebrow">Founder narrative trainer</p>
          <h1>The Founder Identity Game</h1>
        </div>
        <div class="header-actions">
          <button class="mode-button ${state.mode === 'classic' ? 'active' : ''}" data-mode="classic">Cards</button>
          <button class="mode-button ${state.mode === 'anti-bs' ? 'active' : ''}" data-mode="anti-bs">Anti-BS</button>
        </div>
      </header>

      <section class="control-strip" aria-label="Game controls">
        <div class="timer" data-timer>
          <strong>${state.remaining}</strong>
          <span>${state.isRunning ? 'speaking' : 'ready'}</span>
        </div>
        <div class="control-group">
          <button class="primary-button" data-action="deal">New round</button>
          <button class="secondary-button" data-action="start">${state.isRunning ? 'Running' : 'Start'}</button>
          <button class="secondary-button" data-action="pause">Pause</button>
          <button class="secondary-button" data-action="reset">Reset</button>
        </div>
        <label class="select-label">
          Time
          <select data-duration>
            <option value="30" ${state.duration === 30 ? 'selected' : ''}>30 sec</option>
            <option value="60" ${state.duration === 60 ? 'selected' : ''}>60 sec</option>
            <option value="120" ${state.duration === 120 ? 'selected' : ''}>2 min</option>
          </select>
        </label>
        <label class="toggle-label">
          <input type="checkbox" data-constraint-toggle ${state.includeConstraint ? 'checked' : ''} />
          Constraint card
        </label>
      </section>

      ${state.mode === 'classic' ? renderClassicRound() : renderAntiBsRound()}

      <section class="scoreboard">
        <div>
          <p class="section-label">Self-check</p>
          <h2 data-score-summary>No score yet</h2>
        </div>
        <label>
          Clarity
          <input type="range" min="0" max="5" value="${state.scores.clarity}" data-score="clarity" />
        </label>
        <label>
          Coherence
          <input type="range" min="0" max="5" value="${state.scores.coherence}" data-score="coherence" />
        </label>
        <label>
          Memorability
          <input type="range" min="0" max="5" value="${state.scores.memorability}" data-score="memorability" />
        </label>
        <button class="primary-button" data-action="complete">Complete round</button>
      </section>

      <aside class="history-panel">
        <div>
          <p class="section-label">Recent rounds</p>
          <h2>Practice log</h2>
        </div>
        <ul>${renderHistory()}</ul>
      </aside>
    </main>
  `;

  renderTimer();
  renderScoreSummary();
}

app.addEventListener('click', (event) => {
  const action = event.target.closest('[data-action]')?.dataset.action;
  const mode = event.target.closest('[data-mode]')?.dataset.mode;
  const lock = event.target.closest('[data-lock]')?.dataset.lock;

  if (action === 'deal') createRound();
  if (action === 'start') startTimer();
  if (action === 'pause') pauseTimer();
  if (action === 'reset') resetTimer() || render();
  if (action === 'complete') completeRound();
  if (mode) setMode(mode);
  if (lock) toggleLock(lock);
});

app.addEventListener('input', (event) => {
  if (event.target.matches('[data-score]')) {
    setScore(event.target.dataset.score, event.target.value);
  }
});

app.addEventListener('change', (event) => {
  if (event.target.matches('[data-duration]')) {
    setDuration(event.target.value);
  }

  if (event.target.matches('[data-constraint-toggle]')) {
    state.includeConstraint = event.target.checked;
    render();
  }
});

createRound();
