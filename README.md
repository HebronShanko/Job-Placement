# Job-Placement
Here is where I will explain my Live Project Course and give a brief summary on what the two week sprit initialed. 



This Live project was built using the Django framework. This Live project tasked my fellow students and I the resposibility to create an interactive website for managing one's collections of things related to various hobbies, as well as API and Data Scraped content for those hobbies. This Live Project, that was in done in a two week sprint gave myself a chance to glance at what working in teams in the tech industry could be, we exerciesed management methodologies, working on a small piece of a larger project in the middle of its lifecycle and other skills sets that are vital in the technology professional. 


To focus more on the coding aspect, in the next paragraps or two I will provide code summary of the Live Projects and explains the stories and the solution I came up with using Python and Django Framework. 


One of the talk was creating a model that included an objects manager for accessing the database:


        from django.db import models

        class Player(models.Model):
            objects = models.Manager
            first_name = models.CharField(max_length=30)
            last_name = models.CharField(max_length=30)
            what_position = models.CharField(max_length=30)
            what_number = models.IntegerField()
            strong_feet = models.CharField(max_length=30)

            def __str__(self):
                return self.first_name


        class Team(models.Model):
          team_name = models.ForeignKey(Player, on_delete=models.CASCADE)
          team_location = models.CharField(max_length=30)
          head_coach = models.CharField(max_length=30)
          team_mascot = models.CharField(max_length=30)
          team_record = models.IntegerField()

          objects = models.Manager()

          def __str__(self):
              return self.team_name



Then we had to add a views function that renders the create page and utilizes the model form to save the collection item to the database.


        # Create your views here.
        def StatCheckHome(request):
            return render(request, 'StatCheck/StatCheckHOME.html')



On the the next story, I was tasked to add in a function that gets all the items from the database and sends them to the template to display the list of items from the database, with some or all of the fields. 


        def CheckPlayer(request):
            if request.method == "POST":
                playerForm = PlayerForm(request.POST, request.FILES)
                if playerForm.is_valid():
                    playerForm.save()
                    messages.success(request, ('Your player was successfully added!'))
                else:
                    messages.error(request, 'Error saving form')

                return redirect("StatCheckPLAYER")
            playerForm = PlayerForm()
            Stat = Player.objects.all()
            return render(request, "StatCheck/StatCheckPLAYER.html", context={'playerForm': playerForm, 'Stat': Stat})


        def CheckTeam(request):
            if request.method == "POST":
                teamForm = TeamForm(request.POST, request.FILES)
                if teamForm.is_valid():
                    teamForm.save()
                    messages.success(request, ('Your team was successfully added!'))
                else:
                    messages.error(request, 'Error saving form')

                return redirect("StatCheckTEAM")
            teamForm = TeamForm()
            Stat = Team.objects.all()
            return render(request, "StatCheck/StatCheckTEAM.html", context={'teamForm': teamForm, 'Stat': Stat})


Lastly, I created a views function that will find a single item with link for each item from the database and send it to the template where iteams are displayed all the details of the item on the details page.


        def player_display(request):
            player_display = Player.objects.all()
            content = {'player_display': player_display}
            return render(request, 'StatCheck/StatCheckDISPLAY.html', content)

        def CheckDetail(request, pk):
            CheckDetail = get_object_or_404(Player, pk=pk)
            return render(request, 'StatCheck/StatCheckDETAIL.html', {'CheckDetail': CheckDetail})
