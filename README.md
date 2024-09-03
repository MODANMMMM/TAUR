from flask import Flask, render_template_string, request
import io
import sys

app = Flask(__name__)

# Your GuessingGame class
class GuessingGame:
    def __init__(self):
        # possible answers
        self.possible_answers = [
            {"answer": "Standard ‘0’ (STD) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
            "image_path": "STD.png"},
            {"answer": "Standard ‘0’ (STD1) on MT4 & MT5 with 0% rebate, Not Swap Free, 4 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 4},
            "image_path": "STD1.png"},
            {"answer": "Standard ‘0’ (STD2) on MT4 & MT5 with 0% rebate, Not Swap Free, 3 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 3},
 "image_path": "STD2.png"},
            {"answer": "Standard ‘0’ (STD3) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "STD3.png"},
            {"answer": "Standard ‘0’ (STD4) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "STD4.png"},
            {"answer": "Standard ‘0’ (STD5) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "STD5.png"},
            {"answer": "Standard ‘0’ (STD6) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "STD6.png"},
            {"answer": "Standard ‘0’ (STD7) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "STD7.png"},
            {"answer": "Standard ‘0’ (STD8) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "STD8.png"},
            {"answer": "Standard ‘0’ (STD9) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "STD9.png"},
            {"answer": "Standard ‘0’ (STD10) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "STD10.png"},
            {"answer": "Standard ‘0’ (STD12) on MT4 & MT5 with 0% rebate, Not Swap Free, 3 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 3},
 "image_path": "STD12.png"},
            {"answer": "Standard ‘0’ (STD13) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "STD13.png"},
            {"answer": "Standard ‘0’ (ISTD1_SwapFree_USD) on MT4 & MT5 with 0% rebate, Swap Free, 2 tiers",
            "attributes": {"account_type": "s2d", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": True, "tier": 2},
 "image_path": "ISTD1_SwapFree_USD.png"},
            {"answer": "Standard ‘0’ (ISTD2_SwapFree_USD) on MT4 & MT5 with 0% rebate, Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": True, "tier": 2},
 "image_path": "ISTD2_SwapFree_USD.png"},
            {"answer": "Standard ‘0’ (ISTD4_SwapFree_USD) on MT4 & MT5 with 0% rebate, Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": True, "tier": 1},
 "image_path": "ISTD4_SwapFree_USD.png"},
            {"answer": "Standard ‘0’ (STD05M) on MT4 & MT5 with 5% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 5, "swap_free": False, "tier": 1},
 "image_path": "STD05M.png"},
            {"answer": "Standard ‘0’ (STD05M1) on MT4 & MT5 with 5% rebate, Not Swap Free, 3 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 5, "swap_free": False, "tier": 3},
 "image_path": "STD05M1.png"},
            {"answer": "Standard ‘0’ (STD05M2) on MT4 & MT5 with 5% rebate, Not Swap Free, 4 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 5, "swap_free": False, "tier": 4},
 "image_path": "STD05M2.png"},
            {"answer": "Standard ‘0’ (STD05M3) on MT4 & MT5 with 5% rebate, Not Swap Free, 4 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 5, "swap_free": False, "tier": 4},
 "image_path": "STD05M3.png"},
            {"answer": "Standard ‘0’ (STD10Me1) on MT4 & MT5 with 10% rebate, Not Swap Free, 2 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 10, "swap_free": False, "tier": 2},
 "image_path": "STD10Me1.png"},
            {"answer": "Standard ‘0’ (STD10M) on MT4 & MT5 with 10% rebate, Not Swap Free, 3 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 10, "swap_free": False, "tier": 3},
 "image_path": "STD10M.png"},
            {"answer": "Standard ‘0’ (STD20M) on MT4 & MT5 with 20% rebate, Not Swap Free, 1 tiers",
            "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 20, "swap_free": False, "tier": 1},
 "image_path": "STD20M.png"},
            {"answer": "Standard ‘0’ (STD20M1) on MT4 & MT5 with 20% rebate, Not Swap Free, 4 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 20, "swap_free": False, "tier": 4},
 "image_path": "STD20M1.png"},
            {"answer": "Standard ‘0’ (STD20M2) on MT4 & MT5 with 20% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 20, "swap_free": False, "tier": 2},
 "image_path": "STD20M2.png"},
            {"answer": "Standard ‘0’ (STD10M1) on MT4 & MT5 with 10% rebate, Not Swap Free, 4 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 10, "swap_free": False, "tier": 4},
 "image_path": "STD10M1.png"},
            {"answer": "Standard ‘0’ (STD10Me) on MT4 & MT5 with 10% rebate, Not Swap Free, 3 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 10, "swap_free": False, "tier": 3},
 "image_path": "STD10Me.png"},
            {"answer": "Standard ‘0’ (STD30M1) on MT4 & MT5 with 30% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 30, "swap_free": False, "tier": 2},
 "image_path": "STD30M1.png"},
            {"answer": "Standard ‘0’ (STD30M) on MT4 & MT5 with 30% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 30, "swap_free": False, "tier": 1},
 "image_path": "STD30M.png"},
            {"answer": "Standard ‘0’ (STD30M2) on MT4 & MT5 with 30% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "std", "platform": ["mt4", "mt5"], "rebate": 30, "swap_free": False, "tier": 2},
 "image_path": "STD30M2.png"},
            {"answer": "Pro ‘0’ (PRO) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "pro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "PRO.png"},
            {"answer": "Pro ‘0’ (PRO1) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "pro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "PRO1.png"},
            {"answer": "Pro ‘0’ (PRO10C) on MT4 & MT5 with 10% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "pro", "platform": ["mt4", "mt5"], "rebate": 10, "swap_free": False, "tier": 1},
 "image_path": "PRO10C.png"},
            {"answer": "Pro ‘0’ (PRO15C) on MT4 & MT5 with 15% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "pro", "platform": ["mt4", "mt5"], "rebate": 15, "swap_free": False, "tier": 1},
 "image_path": "PRO15C.png"},
            {"answer": "Raw Silver (SVR) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "SVR.png"},
            {"answer": "Raw Silver (SVR1) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "SVR1.png"},
            {"answer": "Raw Silver (SVR2) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "SVR2.png"},
            {"answer": "Raw Silver (SVR3) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "SVR3.png"},
            {"answer": "Raw Silver (SVR4) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "SVR4.png"},
            {"answer": "Raw Silver (SVR5) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "SVR5.png"},
            {"answer": "Raw Silver (SVR6) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "SVR6.png"},
            {"answer": "Raw Silver (SVR7) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "SVR7.png"},
            {"answer": "Raw Silver (SVR8) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "svr", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "SVR8.png"},
            {"answer": "Raw Bronze (BRO) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "BRO.png"},
            {"answer": "Raw Bronze (BRO1) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "BRO1.png"},
            {"answer": "Raw Bronze (BRO2) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "BRO2.png"},
            {"answer": "Raw Bronze (BRO3) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "BRO3.png"},
            {"answer": "Raw Bronze (BRO4) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "BRO4.png"},
            {"answer": "Raw Bronze (BRO1C) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "BRO1C.png"},
            {"answer": "Raw Bronze (BRO1C1) on MT4 & MT5 with 0% rebate, Not Swap Free, 3 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 3},
 "image_path": "BRO1C1.png"},
            {"answer": "Raw Bronze (BRO1C2) on MT4 & MT5 with 0% rebate, Not Swap Free, 2 tiers",
             "attributes": {"account_type": "bro", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 2},
 "image_path": "BRO1C2.png"},
            {"answer": "Raw Gold (GLD) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "gld", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "GLD.png"},
            {"answer": "Cent (USC) on MT4 & MT5 with 0% rebate, Not Swap Free, 1 tiers",
             "attributes": {"account_type": "usc", "platform": ["mt4", "mt5"], "rebate": 0, "swap_free": False, "tier": 1},
 "image_path": "USC.png"}
        ]
        self.filtered_answers = self.possible_answers[:]  # questions for filtering
        self.questions = [
            {
                "question": "Please Select the Type of Account",
                "attribute": "account_type",
                "options": {
                    "1": "std",
                    "2": "pro",
                    "3": "svr",
                    "4": "bro",
                    "5": "gld",
                    "6": "usc",
                    "7": "ap_bundle"
                }
            },
            {
                "question": "Please Select the Type of Platform",
                "attribute": "platform",
                "options": {
                    "1": "mt4",
                    "2": "mt5",
                    "3": "mt4_mt5"
                }
            },
            {
                "question": "Please Select the Mark Up Value(USD)",
                "attribute": "rebate",
                "options": {
                    "1": 0,
                    "2": 5,
                    "3": 10,
                    "4": 15,
                    "5": 20,
                    "6": 30,
                }
            },
            {
                "question": "Do You Need a Swap Free Account？",
                "attribute": "swap_free",
                "options": {
                    "1": True,
                    "2": False
                }
            },
            {
                "question": "Please Select the Number of Tiers",
                "attribute": "tier",
                "options": {
                    "1": 1,
                    "2": 2,
                    "3": 3,
                    "4": 4,
                    "5": 5
                }
            }
        ]

    def ask_question(self, question, attribute, options, user_input):
        answer = user_input.strip()
        if answer in options:
            if attribute == "platform":
                if options[answer] == "mt4":
                    self.filtered_answers = [a for a in self.filtered_answers if "mt4" in a["attributes"]["platform"]]
                elif options[answer] == "mt5":
                    self.filtered_answers = [a for a in self.filtered_answers if "mt5" in a["attributes"]["platform"]]
                elif options[answer] == "mt4_mt5":
                    self.filtered_answers = [a for a in self.filtered_answers if set(["mt4", "mt5"]).issubset(set(a["attributes"]["platform"]))]
            elif attribute == "account_type":
                if options[answer] == "ap_bundle":

                    pass
                else:
                    self.filtered_answers = [a for a in self.filtered_answers if a["attributes"]["account_type"] == options[answer]]
            elif attribute in ["rebate", "tier"]:
                self.filtered_answers = [a for a in self.filtered_answers if a["attributes"][attribute] <= options[answer]]
            else:
                self.filtered_answers = [a for a in self.filtered_answers if a["attributes"][attribute] == options[answer]]

            if len(self.filtered_answers) == 1:
                return f"The Final Account Type, Platform, Rebate, Swap Free Status, Number of Tier is: {self.filtered_answers[0]['answer']}"
        else:
            return "Invalid Value, Please Type In the Number Provided."

        return None

    def start_game(self, user_inputs):
        output_list = []  # initialise list

        for i, q in enumerate(self.questions):
            if len(self.filtered_answers) > 1:
                result = self.ask_question(q["question"], q["attribute"], q["options"], user_inputs[i])
                if result:
                    output_list.append((result, None))  # add to list while have answer
                    break
            else:
                break

        if len(self.filtered_answers) == 1:
            answer = self.filtered_answers[0]
            output_list.append((answer["answer"], answer.get('image_path')))  # single output
        elif len(self.filtered_answers) > 1:
            for item in self.filtered_answers:
                output_list.append((item["answer"], item.get('image_path')))  # many output
        else:
            output_list.append(("Sorry, I cannot find an answer that fits the requirements", None))

        return output_list  # return to list


@app.route('/', methods=['GET', 'POST'])
def console():
    output_list = []  # initialise list
    if request.method == 'POST':
        game = GuessingGame()
        user_inputs = [
            request.form.get('account_type'),
            request.form.get('platform'),
            request.form.get('rebate'),
            request.form.get('swap_free'),
            request.form.get('tier')
        ]
        output_list = game.start_game(user_inputs)  # get the list to picture path

    html = '''
    <!doctype html>
    <html lang="en">
      <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <title>Filter Question</title>
      </head>
      <body>
        <div style="margin: 50px;">
          <h1>Filter Question</h1>
          <form method="post">
            <label>Please Select the Type of Account</label><br>
            <select name="account_type">
                <option value="1">std</option>
                <option value="2">pro</option>
                <option value="3">svr</option>
                <option value="4">bro</option>
                <option value="5">gld</option>
                <option value="6">usc</option>
                <option value="7">ap_bundle</option>
            </select><br><br>

            <label>Please Select the Type of Platform</label><br>
            <select name="platform">
                <option value="1">MT4</option>
                <option value="2">MT5</option>
                <option value="3">MT4&MT5</option>
            </select><br><br>

            <label>Please Select the Mark Up Value (USD)</label><br>
            <select name="rebate">
                <option value="1">0</option>
                <option value="2">5</option>
                <option value="3">10</option>
                <option value="4">15</option>
                <option value="5">20</option>
                <option value="6">30</option>
            </select><br><br>

            <label>Do You Need a Swap Free Account?</label><br>
            <select name="swap_free">
                <option value="1">Yes</option>
                <option value="2">No</option>
            </select><br><br>

            <label>Please Select the Number of Tiers</label><br>
            <select name="tier">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
            </select><br><br>

            <input type="submit" value="Submit">
          </form>
          {% for answer, image in output_list %}
            <p>{{ answer }}</p>
            {% if image %}
              <img src="{{ url_for('static', filename='images/' + image) }}" alt="Result Image">
            {% endif %}
          {% endfor %}
        </div>
      </body>
    </html>
    '''
    return render_template_string(html, output_list=output_list)  # pass list to website


if __name__ == '__main__':
    app.run(debug=True)
