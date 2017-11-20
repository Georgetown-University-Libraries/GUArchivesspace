# Local Overrides for our AS Repo

## Host Specific Entries

    AppConfig[:db_url] = "jdbc:mysql:..."
    
    # Instance URL's
    AppConfig[:frontend_proxy_url] = "https://staff.findingaids.library.georgetown.edu"
    AppConfig[:public_proxy_url] = "https://findingaids.library.georgetown.edu"
    
## General Oveerrides

    # Allow an unauthenticated user to create an account
    AppConfig[:allow_user_registration] = false
    
    # General Settings
    AppConfig[:backend_log_level] = "info"
    AppConfig[:indexer_records_per_thread] = 250
    AppConfig[:pui_indexer_records_per_thread] = 250
    AppConfig[:pui_indexer_thread_count] = 1
    
    # Search Customizations
    AppConfig[:pui_search_results_page_size] = 20
    
    # Control Top Level Navigation Options
    AppConfig[:pui_hide][:digital_objects] = true
    AppConfig[:pui_hide][:accessions] = true
    AppConfig[:pui_hide][:classifications] = true
    
    # Megaphone Button Link Override
    AppConfig[:pui_page_actions_request] = false
    AppConfig[:pui_page_custom_actions] << {
       'record_type' => ['resource', 'archival_object', 'accession', 'digital_object'], # the jsonmodel type to show for
       'label' => 'Request', # the I18n path for the action button
       'icon' => 'fa-bullhorn', # the font-awesome icon CSS class
       'url_proc' => proc {|record| 'https://www.library.georgetown.edu/special-collections/using/requests/information?uri='+record.uri},
    }
    
    # Change Inherited Date Behavior
    AppConfig[:record_inheritance] = {
      :archival_object => {
        :inherited_fields => [
          {
            :property => 'title',
            :inherit_directly => true
          },
          {
            :property => 'component_id',
            :inherit_directly => false
          },
          {
            :property => 'language',
            :inherit_directly => true
          },
          {
            :property => 'dates',
            :inherit_directly => false
          },
          {
            :property => 'extents',
            :inherit_directly => false
          },
          {
            :property => 'linked_agents',
            :inherit_if => proc {|json| json.select {|j| j['role'] == 'creator'} },
            :inherit_directly => false
          },
          {
            :property => 'notes',
            :inherit_if => proc {|json| json.select {|j| j['type'] == 'accessrestrict'} },
            :inherit_directly => true
          },
          {
            :property => 'notes',
            :inherit_if => proc {|json| json.select {|j| j['type'] == 'scopecontent'} },
            :inherit_directly => false
          },
          {
            :property => 'notes',
            :inherit_if => proc {|json| json.select {|j| j['type'] == 'langmaterial'} },
            :inherit_directly => false
          },
        ]
      }
    }
    
    # OAI Config
    AppConfig[:oai_url] = "http://localhost:8082"
    AppConfig[:oai_repository_name] = 'Georgetown Archival Resources OAI Provider'
    AppConfig[:oai_proxy_url] = 'https://findingaids-test.library.georgetown.edu/oai'
    AppConfig[:oai_record_prefix] = 'oai:archivesspace'
    AppConfig[:oai_admin_email] = 'speccoll@georgetown.edu'
    
    AppConfig[:oai_sets] = {
      'GTA' => {
        :repo_codes => ['GTA'],
        :description => "GTA",
      },
      'GTM' => {
        :repo_codes => ['GTM'],
        :description => "GTM",
      },
      'GTRB' => {
        :repo_codes => ['GTRB'],
        :description => "GTRB",
      },
      'BRL' => {
        :repo_codes => ['BRL'],
        :description => "BRL",
      },
      'Woodstock' => {
        :repo_codes => ['Woodstock'],
        :description => "Woodstock",
      },
      
      'repository_set' => {
        :repo_codes => ['GTA','GTM','GTRB','BRL','Woodstock'],
        :description => "A set of one or more repositories",
      }
    }
    
    
    