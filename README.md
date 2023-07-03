# MothershipConcept
A bot that uses the mothership and a narrow Ai bot carrier as an analogy.

### Proof Of Concept
~~~ruby
module Mothership
  class BotCarrier
    def self.chatbot
      require "programr"

      brains = Dir.glob("_imaginedpath/brains/*")

      robot = ProgramR::Facade.new
      robot.learn(brains)

      begin
        print "YOU >> "
        s = STDIN.gets.chomp
  
        if s != 'quit'
          reaction = robot.get_reaction(s)
          STDOUT.puts "CHARLOTTE << #{reaction}"
        end
      end while s != 'quit'
    end

    def self.outcome_evaluator # Make sure that story elements are kept on seperate lines.
      character_fate = File.readlines("_narratives/outcomes/character_fates.txt")
      dating_outcome = File.readlines("_narratives/outcomes/dating_outcomes.txt")

      character_fate_pl = File.readlines("_narratives/outcomes/reasoning/character_fates.txt")
      dating_outcome_pl = File.readlines("_narratives/outcomes/reasoning/dating_outcomes.txt")

      open("_imaginedpath/prolog/nuetral_outcome.pl", "w") { |f|
        segment_1 = character_fate_pl[1].strip
        segment_2 = dating_outcome_pl[0].strip

        f.puts "#{segment_1}."
        f.puts "#{segment_1} :- #{segment_2}"
      }

      # Imagined a compromise path that is neither ideal or tragic.
      open("_imaginedpath/brains/nuetral_outcome.aiml", "w") { |f|
        segment_1 = character_fate[1].strip
        segment_2 = dating_outcome[0].strip

        aiml = "<?xml version = '1.0' encoding = 'UTF-8'?>
<aiml version = '1.0.1' encoding = 'UTF-8'>
  <category>
    <pattern>What is the least good outcome that is not the worst?</pattern>
    <template>#{segment_1} #{segment_2}
      <system>swipl outcome_consultant.pl</system>
    </template>
  </category>
</aiml>"

        f.puts aiml
      }
    end

    def self.self_evaluate
      require "naive_bayes"

      outcomes = NaiveBayes.new(:worst, :nuetral, :best)

      # Train on outcome possibilities.
      outcomes.train(:worst,    "[ charlotte dies ] [ never dated player ]",   "worst")
      outcomes.train(:nuetral,        "[ charlotte dies ] [ dated player ]", "nuetral")
      outcomes.train(:nuetral, "[ charlotte lives ] [ never dated player ]", "nuetral")
      outcomes.train(:best,          "[ charlotte lives ] [ dated player ]",    "best")

      current_outcomes = File.readlines("_imaginedpath/outcomes/nuetral_outcome.txt")

      loop_limit = current_outcomes.size.to_i

      row = 0

      loop_limit.times do
        result = outcomes.classify(*current_outcomes[row])

        puts "For outcome: #{current_outcome[row]}, the current outcome is: #{result[0]}."

        row = row + 1
      end
    end
  end
end

initializer      = File.read("_data/ainput/input.txt")
function         = File.readlines("_functionlist/functions/activities.txt")
current_function = function[initializer]

if    current_function == "chatbot"
  FunctionList::Bot.chatbot
elsif current_function == "outcome evaluator"
  FunctionList::Bot.outcome_evaluator
elsif current_function == "reasoning"
  FunctionList::Bot.reasoning
else
  puts "Function not yet defined..."
end
~~~
