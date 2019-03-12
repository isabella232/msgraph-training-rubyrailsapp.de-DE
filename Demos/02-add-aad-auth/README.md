# <a name="completed-module-add-azure-ad-authentication"></a><span data-ttu-id="2792c-101">Abgeschlossenes Modul: Hinzufügen der Azure AD-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="2792c-101">Completed module: Add Azure AD authentication</span></span>

<span data-ttu-id="2792c-102">Die Version des Projekts in diesem Verzeichnis spiegelt das Abschließen des Lernprogramms durch [Hinzufügen der Azure AD-Authentifizierung](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=3)wider.</span><span class="sxs-lookup"><span data-stu-id="2792c-102">The version of the project in this directory reflects completing the tutorial up through [Add Azure AD authentication](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=3).</span></span> <span data-ttu-id="2792c-103">Wenn Sie diese Version des Projekts verwenden, müssen Sie den Rest des Lernprogramms beginnend beim [Abrufen von Kalenderdaten](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=4)abschließen.</span><span class="sxs-lookup"><span data-stu-id="2792c-103">If you use this version of the project, you need to complete the rest of the tutorial starting at [Get calendar data](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=4).</span></span>

> <span data-ttu-id="2792c-104">**Hinweis:** Es wird davon ausgegangen, dass Sie bereits eine Anwendung im App-Registrierungs Portal registriert haben, wie in [Registrieren der APP im Portal](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=2)angegeben.</span><span class="sxs-lookup"><span data-stu-id="2792c-104">**Note:** It is assumed that you have already registered an application in the app registration portal as specified in [Register the app in the portal](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=2).</span></span> <span data-ttu-id="2792c-105">Sie müssen diese Version des Beispiels wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="2792c-105">You need to configure this version of the sample as follows:</span></span>
>
> 1. <span data-ttu-id="2792c-106">Benennen Sie `./config/oauth_environment_variables.rb.example` die Datei `oauth_environment_variables.rb`in um.</span><span class="sxs-lookup"><span data-stu-id="2792c-106">Rename the `./config/oauth_environment_variables.rb.example` file to `oauth_environment_variables.rb`.</span></span>
> 1. <span data-ttu-id="2792c-107">Bearbeiten Sie `oauth_environment_variables.rb` die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="2792c-107">Edit the `oauth_environment_variables.rb` file and make the following changes.</span></span>
>     1. <span data-ttu-id="2792c-108">Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="2792c-108">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
>     1. <span data-ttu-id="2792c-109">Ersetzen `YOUR APP PASSWORD HERE` Sie durch das Kennwort, das Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="2792c-109">Replace `YOUR APP PASSWORD HERE` with the password you got from the App Registration Portal.</span></span>