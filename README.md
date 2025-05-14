[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/NUTiSmoa)
using System;
using System.Collections.Generic;
using System.Linq;
using System.Media;
using System.Threading;

namespace EchoChatbot
{
    class Program
    {
        static Random r = new Random(); 
        static UserProfile user = new UserProfile();
        static Dictionary<string, Topic> topics = new Dictionary<string, Topic>(StringComparer.OrdinalIgnoreCase)
        {
            ["password"] = new Topic(
                "Password Security",
                "Strong passwerds help keep ur data safe from hackers and bad peepol.",
                new List<string>
                {
                    "Use diff passwords for every site or app.",
                    "Dont put ur name or birth in ur pass.",
                    "Use a password thingy (manager) if u forget passwords alot."
                },
                new List<string> { "password", "pass", "credential" }),
 ["phishing"] = new Topic(
                "Phishing Attacks",
                "phishing is wen scammers trick u to click links or give infomation.",
                new List<string>
                {
                    "Dont click dodgy looking links.",
                    "Check sender email before reply.",
                    "Always tell ur IT guy if u see sus stuff."
                },
                new List<string> { "phish", "scam", "fraud", "link" }),

  ["privacy"] = new Topic(
                "Privacy Protection",
                "Keep ur personal infomashun private on the internet.",
                new List<string>
                {
                    "Don't post ur full info online.",
                    "Check ur privacy setings on apps.",
                    "Use incognito or private mode if needed."
                },
                new List<string> { "privacy", "data", "exposure", "personal info" }),

  ["malware"] = new Topic(
                "Malware Awareness",
                "Malware is bad software that do harm to ur computer.",
                new List<string>
                {
                    "Use antivirus and update it.",
                    "Dont download weird files.",
                    "Run scans if things get slow."
                },
                new List<string> { "malware", "virus", "trojan", "worm" }),

  ["firewall"] = new Topic(
                "Firewall Protection",
                "firewalls keep bad trafic out of ur network.",
                new List<string>
                {
                    "Turn on ur firewall (its in settings).",
                    "Dont let unknown apps through.",
                    "It blocks hackers a lot of times."
                },
                new List<string> { "firewall", "network", "access" }),

  ["mfa"] = new Topic(
                "Multi-Factor Authentication",
                "MFA is like extra secrity if ur password is stolen.",
                new List<string>
                {
                    "Turn on MFA on emails and banks.",
                    "Apps like Authy or Google Auth are better than SMS.",
                    "Even if u lose password, MFA helps stop hackers."
                },
                new List<string> { "mfa", "two factor", "2fa", "authentication" })
        };

  static void Main()
        {
            DisplayEchoLogo();
            PlayIntroSound();
            ShowWelcome();
            user.Name = GetName();
            RunChat();
        }

  static void RunChat()
        {
            Console.Clear();
            DisplayEchoLogo();
            Console.WriteLine($"\nHey {user.Name}! Im ECHO. Just ask me stuff about cyber. Type 'exit' to go.");
            Console.WriteLine("Go ahead say something...\n");

  while (true)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.Write("You: ");
                Console.ResetColor();

  string input = Console.ReadLine()?.ToLower();
                if (string.IsNullOrWhiteSpace(input)) continue;
                if (input == "exit") break;

  string reply = GetReply(input);
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.Write("ECHO: ");
                Console.ResetColor();
                SlowType(reply + "\n");
            }
        }

  static string GetReply(string input)
        {
            string mood = GetMood(input);
            string moodMsg = MoodMessage(mood);

  foreach (var topic in topics.Values)
            {
                if (topic.Keywords.Any(k => input.Contains(k)))
                {
                    if (!user.Interests.Contains(topic.Title))
                        user.Interests.Add(topic.Title);

  return moodMsg + RandomTip(topic);
                }
            }

  if ((input.Contains("remember") || input.Contains("remind")) && user.Interests.Any())
            {
                return $"U were curious about: {string.Join(", ", user.Interests)}";
            }

  return "Hmm.. I don't get that. Ask about security or try something else!";
        }

  static string RandomTip(Topic topic)
        {
            return topic.Responses[r.Next(topic.Responses.Count)];
        }

  static string GetMood(string input)
        {
            var moods = new Dictionary<string, List<string>>
            {
                {"worried", new List<string> { "worried", "scared", "nervous" }},
                {"curious", new List<string> { "curious", "wonder", "interested" }},
                {"confused", new List<string> { "confused", "unsure", "don't understand" }}
            };

  foreach (var m in moods)
            {
                if (m.Value.Any(k => input.Contains(k)))
                    return m.Key;
            }

  return "neutral";
        }

  static string MoodMessage(string mood)
        {
            return mood switch
            {
                "worried" => "Dont stress, its ok. ",
                "curious" => "Nice question! ",
                "confused" => "I'll try to make it eise. ",
                _ => ""
            };
        }

  static void DisplayEchoLogo()
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine(@"
          _____                    _____                    _____                   _______         
         /\    \                  /\    \                  /\    \                 /::\    \        
        /::\    \                /::\____\                /::\    \               /::::\    \       
       /::::\    \              /:::/    /               /::::\    \             /::::::\    \      
      /::::::\    \            /:::/    /               /::::::\    \           /::::::::\    \     
     /:::/\:::\    \          /:::/    /               /:::/\:::\    \         /:::/~~\:::\    \    
    /:::/__\:::\    \        /:::/____/               /:::/  \:::\    \       /:::/    \:::\    \   
   /::::\   \:::\    \      /::::\    \              /:::/    \:::\    \     /:::/    / \:::\    \  
  /::::::\   \:::\    \    /::::::\    \   _____    /:::/    / \:::\    \   /:::/____/   \:::\____\ 
 /:::/\:::\   \:::\    \  /:::/\:::\    \ /\    \  /:::/    /   \:::\    \ |:::|    |     |:::|    |
/:::/__\:::\   \:::\____\/:::/  \:::\    /::\____\/:::/____/     \:::\____\|:::|____|     |:::|    |
\:::\   \:::\   \::/    /\::/    \:::\  /:::/    /\:::\    \      \::/    / \:::\    \   /:::/    / 
 \:::\   \:::\   \/____/  \/____/ \:::\/:::/    /  \:::\    \      \/____/   \:::\    \ /:::/    /  
  \:::\   \:::\    \               \::::::/    /    \:::\    \                \:::\    /:::/    /   
   \:::\   \:::\____\               \::::/    /      \:::\    \                \:::\__/:::/    /    
    \:::\   \::/    /               /:::/    /        \:::\    \                \::::::::/    /     
     \:::\   \/____/               /:::/    /          \:::\    \                \::::::/    /      
      \:::\    \                  /:::/    /            \:::\    \                \::::/    /       
       \:::\____\                /:::/    /              \:::\____\                \::/____/        
        \::/    /                \::/    /                \::/    /                 ~~              
         \/____/                  \/____/                  \/____/                                  
            ");
            Console.ResetColor();
        }

  static void PlayIntroSound()
        {
            try
            {
                using var player = new SoundPlayer("greeting.wav");
                player.Play();
            }
            catch (Exception ex)
            {
                Console.WriteLine($"cant play sound: {ex.Message}");
            }
        }

  static void ShowWelcome()
        {
            Console.ForegroundColor = ConsoleColor.Green;
            SlowType("\nHello and welcome to ECHO, ur cyber helper bot.");
            Console.ResetColor();
            SlowType("\n--------------------\n");
        }

  static void SlowType(string msg, int delay = 25)
        {
            foreach (char c in msg)
            {
                Console.Write(c);
                Thread.Sleep(delay);
            }
        }

  static string GetName()
        {
            Console.Write("Whats ur name? ");
            string name = Console.ReadLine()?.Trim();
            return string.IsNullOrWhiteSpace(name) ? "Guest" : name;
        }
    }

  class Topic
    {
        public string Title { get; }
        public string Description { get; }
        public List<string> Responses { get; }
        public List<string> Keywords { get; }

  public Topic(string t, string d, List<string> r, List<string> k)
        {
            Title = t;
            Description = d;
            Responses = r;
            Keywords = k;
        }
    }

  class UserProfile
    {
        public string Name { get; set; } = "Guest";
        public List<string> Interests { get; } = new List<string>();
    }
}
