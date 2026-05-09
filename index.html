const storageKey = "workbell.tasks.v1";
const form = document.querySelector("#taskForm");
const titleInput = document.querySelector("#taskTitle");
const noteInput = document.querySelector("#taskNote");
const dateInput = document.querySelector("#taskDate");
const timeInput = document.querySelector("#taskTime");
const list = document.querySelector("#taskList");
const template = document.querySelector("#taskTemplate");
const filterTabs = [...document.querySelectorAll(".filter-tab")];
const notifyButton = document.querySelector("#notifyButton");
const installButton = document.querySelector("#installButton");
const pendingCount = document.querySelector("#pendingCount");
const homePendingCount = document.querySelector("#homePendingCount");
const homePendingList = document.querySelector("#homePendingList");
const nextTask = document.querySelector("#nextTask");
const todayPill = document.querySelector("#todayPill");
const todayStat = document.querySelector("#todayStat");
const overdueStat = document.querySelector("#overdueStat");
const doneStat = document.querySelector("#doneStat");

let tasks = loadTasks();
let deferredInstallPrompt = null;
let activeFilter = "pending";
const timers = new Map();

const dateFormatter = new Intl.DateTimeFormat(undefined, {
  dateStyle: "medium",
  timeStyle: "short",
});

setDefaultDateTime();
todayPill.textContent = new Intl.DateTimeFormat(undefined, {
  weekday: "short",
  day: "numeric",
  month: "short",
}).format(new Date());
render();
scheduleAllReminders();
setInterval(checkDueReminders, 30000);

if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("service-worker.js").catch(() => {});
}

window.addEventListener("beforeinstallprompt", (event) => {
  event.preventDefault();
  deferredInstallPrompt = event;
  installButton.hidden = false;
});

installButton.addEventListener("click", async () => {
  if (!deferredInstallPrompt) return;
  deferredInstallPrompt.prompt();
  await deferredInstallPrompt.userChoice;
  deferredInstallPrompt = null;
  installButton.hidden = true;
});

notifyButton.addEventListener("click", async () => {
  if (!("Notification" in window)) {
    alert("This browser does not support notifications.");
    return;
  }
  const permission = await Notification.requestPermission();
  notifyButton.textContent = permission === "granted" ? "Reminders enabled" : "Enable reminders";
  scheduleAllReminders();
});

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const dueAt = new Date(`${dateInput.value}T${timeInput.value}`);
  if (Number.isNaN(dueAt.getTime())) return;

  tasks.push({
    id: crypto.randomUUID(),
    title: titleInput.value.trim(),
    note: noteInput.value.trim(),
    dueAt: dueAt.toISOString(),
    done: false,
    notified: false,
    createdAt: new Date().toISOString(),
  });

  saveTasks();
  form.reset();
  setDefaultDateTime();
  render();
  scheduleAllReminders();
});

filterTabs.forEach((button) => {
  button.addEventListener("click", () => {
    activeFilter = button.dataset.filter;
    filterTabs.forEach((tab) => tab.classList.toggle("active", tab === button));
    render();
  });
});

list.addEventListener("click", (event) => {
  const taskEl = event.target.closest(".task");
  if (!taskEl) return;
  const task = tasks.find((item) => item.id === taskEl.dataset.id);
  if (!task) return;

  if (event.target.closest(".check")) {
    task.done = !task.done;
    task.notified = task.done ? true : false;
  }

  if (event.target.closest(".delete")) {
    tasks = tasks.filter((item) => item.id !== task.id);
  }

  saveTasks();
  render();
  scheduleAllReminders();
});

function loadTasks() {
  try {
    return JSON.parse(localStorage.getItem(storageKey)) ?? [];
  } catch {
    return [];
  }
}

function saveTasks() {
  localStorage.setItem(storageKey, JSON.stringify(tasks));
}

function setDefaultDateTime() {
  const now = new Date();
  now.setMinutes(now.getMinutes() + 15);
  dateInput.value = formatInputDate(now);
  timeInput.value = now.toTimeString().slice(0, 5);
}

function formatInputDate(date) {
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, "0");
  const day = String(date.getDate()).padStart(2, "0");
  return `${year}-${month}-${day}`;
}

function render() {
  const now = new Date();
  const pending = tasks.filter((task) => !task.done);
  const today = tasks.filter((task) => isSameDay(new Date(task.dueAt), now) && !task.done);
  const overdue = pending.filter((task) => new Date(task.dueAt) < now);
  const done = tasks.filter((task) => task.done);

  pendingCount.textContent = `${pending.length} ${pending.length === 1 ? "work" : "works"}`;
  homePendingCount.textContent = pending.length;
  todayStat.textContent = today.length;
  overdueStat.textContent = overdue.length;
  doneStat.textContent = done.length;

  const next = [...pending].sort(sortByDue)[0];
  nextTask.innerHTML = next
    ? `<strong>${escapeHtml(next.title)}</strong><br>${dateFormatter.format(new Date(next.dueAt))}`
    : "No pending work yet.";

  renderHomePending(pending, now);

  list.replaceChildren();
  const visibleTasks = getVisibleTasks().sort(sortByDue);

  if (visibleTasks.length === 0) {
    const empty = document.createElement("div");
    empty.className = "empty";
    empty.textContent = "Nothing here. Add a work reminder above.";
    list.append(empty);
    return;
  }

  for (const task of visibleTasks) {
    const node = template.content.firstElementChild.cloneNode(true);
    const due = new Date(task.dueAt);
    node.dataset.id = task.id;
    node.classList.toggle("done", task.done);
    node.classList.toggle("overdue", !task.done && due < now);
    node.querySelector(".task-status").textContent = getTaskStatus(task, due, now);
    node.querySelector("h3").textContent = task.title;
    node.querySelector(".task-note").textContent = task.note;
    node.querySelector("time").dateTime = task.dueAt;
    node.querySelector("time").textContent = formatDue(due, now);
    node.querySelector(".check").ariaLabel = task.done ? "Mark pending" : "Mark done";
    list.append(node);
  }
}

function renderHomePending(pending, now) {
  homePendingList.replaceChildren();
  const topPending = [...pending].sort(sortByDue).slice(0, 3);

  if (topPending.length === 0) {
    const empty = document.createElement("div");
    empty.className = "home-pending-empty";
    empty.textContent = "No pending college work. Add your next assignment or study reminder.";
    homePendingList.append(empty);
    return;
  }

  for (const task of topPending) {
    const item = document.createElement("article");
    item.className = "home-pending-item";

    const title = document.createElement("strong");
    title.textContent = task.title;

    const due = document.createElement("span");
    due.textContent = formatDue(new Date(task.dueAt), now);

    item.append(title, due);
    homePendingList.append(item);
  }
}

function getVisibleTasks() {
  const now = new Date();
  if (activeFilter === "pending") return tasks.filter((task) => !task.done);
  if (activeFilter === "today") return tasks.filter((task) => !task.done && isSameDay(new Date(task.dueAt), now));
  if (activeFilter === "done") return tasks.filter((task) => task.done);
  return tasks;
}

function sortByDue(a, b) {
  return new Date(a.dueAt) - new Date(b.dueAt);
}

function isSameDay(a, b) {
  return a.getFullYear() === b.getFullYear()
    && a.getMonth() === b.getMonth()
    && a.getDate() === b.getDate();
}

function formatDue(due, now) {
  const label = dateFormatter.format(due);
  if (due < now) return `Overdue - ${label}`;
  if (isSameDay(due, now)) return `Today - ${label}`;
  return label;
}

function getTaskStatus(task, due, now) {
  if (task.done) return "Done";
  if (due < now) return "Overdue";
  if (isSameDay(due, now)) return "Today";
  return "Upcoming";
}

function scheduleAllReminders() {
  for (const timer of timers.values()) clearTimeout(timer);
  timers.clear();

  for (const task of tasks) {
    if (task.done || task.notified) continue;
    const delay = new Date(task.dueAt).getTime() - Date.now();
    if (delay <= 0) continue;
    if (delay > 2147483647) continue;
    timers.set(task.id, setTimeout(() => remind(task.id), delay));
  }
}

function checkDueReminders() {
  let changed = false;
  for (const task of tasks) {
    if (!task.done && !task.notified && new Date(task.dueAt) <= new Date()) {
      remind(task.id);
      changed = true;
    }
  }
  if (changed) {
    saveTasks();
    render();
  }
}

function remind(taskId) {
  const task = tasks.find((item) => item.id === taskId);
  if (!task || task.done || task.notified) return;
  task.notified = true;
  saveTasks();
  render();

  if ("Notification" in window && Notification.permission === "granted") {
    navigator.serviceWorker?.ready
      .then((registration) => registration.showNotification(task.title, {
        body: task.note || "Your work time has arrived.",
        tag: task.id,
        icon: "icons/icon.svg",
        badge: "icons/icon.svg",
      }))
      .catch(() => new Notification(task.title, { body: task.note || "Your work time has arrived." }));
  }
}

function escapeHtml(value) {
  const div = document.createElement("div");
  div.textContent = value;
  return div.innerHTML;
}
