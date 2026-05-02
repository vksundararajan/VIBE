+++
date = '2024-12-23'
draft = false
title = 'eJPT: My Experience'
tags = ["cert"]
+++

Hey everyone,

I’m super excited to share that I’ve passed the <mark>eJPT exam</mark> with a score of **94% in my first attempt**. I managed to finish it in 6 hours, and I’ve got some handy tips and personal insights that might help those of you gearing up for the same challenge.

![eJPT](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*x9qSy2pnIywruzaCBBfLQg.png)

## A Little Bit About Me
Before jumping into the specifics, let me give you a bit of background. I’ve been a **software developer** for three years, but I had zero experience in penetration testing before this. However, I decided to give it a shot and prepped for about two months. This goes to show that anyone can master this with the right focus and dedication.

### My Approach to the Exam
Here’s something interesting — I didn’t follow the typical advice of jotting down detailed notes during the exam. Instead, I saved all the crucial info right on the target machines. This tactic helped me stay focused and keep a steady pace throughout the test.

If you think you can complete the task quickly, there’s no need to save notes externally. However, never stop the machine during the process. If you do, it’s over — you’ll lose everything.

Someone on LinkedIn told me that if I could solve all the easy challenges on TryHackMe, I would pass the exam. So, I focused on completing every easy CTF on TryHackMe before taking the exam. But that turned out to be unnecessary.

Even if you feel less confident, <mark>remember you have one more attempt</mark>. Don’t stress too much — just dive in and get through it quickly.

## Working Through the Exam
The exam had **six machines** ready to be tackled. After compromising four of them, I was able to answer all the questions I was asked.

**My advice?** Read each question carefully to make sure you really understand what’s being asked, and then go for it.

### Tips for Smoother Testing

- <mark>Keep Bruteforce Simple</mark> Don’t waste time running large wordlists. Often, <u>clues in the questions help narrow down the passwords</u>. When you’re given four options in the question and asked to choose the correct password, simply copy those four options into a text file and run a brute-force attack. This can save a lot of time.

- <mark>Be Careful with Flags</mark> When dealing with flag.txt questions, <u>double-check the PATH to ensure you’re retrieving the flag from the correct location before submitting</u>. You won’t be able to change your answer later. The flag’s path will be specified in the question, so make sure to extract the flag only from that exact path.

- <mark>Don’t Overthink the Pivot Machines</mark> I didn’t need to do much with the pivot machine. A basic TCP scan was enough to get the job done, so don’t stress over it. I was simply asked which vulnerable service was running on the target machine. When I ran a TCP port scan in Metasploit, it revealed only one service. I submitted that and completed the task — no need for port forwarding or any extra steps.

**NOTE** The pivot process might get a bit tricky, but I didn’t worry about it since there were no questions requiring information from inside the pivot machine. I didn’t even try to compromise it. However, after completing the exam, I found out that marks were deducted because I didn’t demonstrate the pivoting. If I had known this earlier, I would have done the pivoting process. Since no questions directly asked for it, I assumed skipping it was fine.

### Privilege Escalation Made Easy
Getting higher privileges didn’t require anything too tricky. I got direct access to NT authority on two machines, cracked the Administrator password via SMB bruteforce on another, and exploited SUID on the last one. Look for the straightforward solutions; they’re there.

## Reflections on the Exam Difficulty
Honestly, I thought the exam would be tougher. It turned out to be **easier than the labs in INE**. So, my final piece of advice? Prepare well, but don’t psych yourself out thinking it’s going to be super hard.

### A Word to Future Test-Takers
Just dive into the material, practice a lot, and don’t sweat it too much. The eJPT is totally doable if you approach it with a calm and collected mindset.

Best of luck to all of you! Keep hacking and keep learning!

---

Feel free to contact me on <span class="tags"><a target="_blank" href="https://www.linkedin.com/in/vigneshwar-sundararajan-07a2a5185/">LinkedIn</a></span> if you have any questions. I’ll do my best to respond quickly. Thank you for investing your time in reading my blog!