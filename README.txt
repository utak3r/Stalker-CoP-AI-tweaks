             _____
     /\     |_   _|
    /  \      | |
   / /\ \     | |
  / ____ \ _ _| |_ _
 /_/    \_(_)_____(_)           _
       |__   __|call of pripyat| |
          | |_      _____  __ _| | _____
          | \ \ /\ / / _ \/ _` | |/ / __|
          | |\ V  V /  __/ (_| |   <\__ \
          |_| \_/\_/ \___|\__,_|_|\_\___/ 2.04

by Alundaio

*About*
	AI Tweaks is a compilation of AI improvements, additions, bugfixes and just overall tweaking of scripts and configuration files
	to improve gameplay. It started off as a project to improve stealth gameplay into something I feel is now a game neccessity.

*Core Features*
	NPCs properly exit danger mode on some instances where they would freeze or be stuck in danger mode endlessly in vanilla.

	Sound is detected by NPCs due to increased danger ignore distance for enemy sounds.

	NPCs will exit combat if enemy is hidden long enough. (Times based on community and rank. Editiable in xr_combat_ignore config)

	Bullets flying past NPCs head cause danger for a short time.

	NPC properly help wounded stalkers. Also optional changes to animation and ability for stalkers to help wounded in combat.

	NPC will eat medkits and bandages when wounded (If they have them.)

	NPC eat, drink and have a chance to sleep at night.

	No more strange behavior on the borders of no combat zones.

	NPC gets a range bonus when looking into scopes (From altered code of AI Additions by Rulix)

	Custom kill wounded scheme.

	Custom Item Gathering scheme with custom behavior when artifacts are detected.

	NPCs sprint to cover during an emission. Ignore combat until cover is reached; Gives the illusion of fighting over cover.

	And much more!

	- Optional: Increased switch distance

	- Optional: The most impressive character behaviors and profiles from Massive Simulation Overhaul created by Trojanuch.

	- Optional: Tweaked Creatures (Use either MSO-lite or this)

	- Optional: NPCs level-to-level traveling. Travelling out of Pripyat has been disabled.

	- Optional: Insert these values into the game console (Warning: makes the game difficult!) NPCs can hit moving targets properly.
		ai_aim_max_angle 		25.0
		ai_aim_min_angle 		20.0
		ai_aim_min_speed 		2.50
		ai_aim_predict_time 	0.28

	- Bug Fixes:
		- xr_combat_ignore.script: Enemey evaluator re-written to eliminate a hang-time issue in vanilla.
		- xr_danger.script: Almost entirely re-written to eliminate hang-time issues in vanilla and bugs that would break quests.
		- release_body_manager.script: Idle after death decreased and body max count increased.
		- sim_squad_script.script: Fixes all ltx script that use the wrong value to change a squads relation.
		- game_relations.script: Fixes all ltx script that use the wrong value to change a squads relation.
		- game_relations.ltx: Fixed improper values for NPC rank. Fixed ALOT of issues with scripts that did checks for rank.
		- bind_crow.script: Fixes excessive spawning
		- pri_a16_kovalski_start.ltx:  Kovalski cutscene fix
		- labx8_burer: immunity fix
		- zat_b38_stalker_cop: Fixes bloodsucker lair quest on higher Switch Distances

*Script Changes*
	- ai_tweaks:
		- created callbacks into a separate script to lessen the mess of xr_motivator and bind_stalker.
		- Should make future merging a bit easier.
		- Can enable ammo usage by NPCs in configs\ai_tweaks\ai_tweaks.ltx
		- NPCs get a Range bonus and FOV penalty when using Scopes

	- move_mgr:
		- sit, sit_ass and sit_knee states for walker scheme will randomly play eat_kolbasa, eat_energy_drink, eat_bread and eat_vodka.
			- It's actually set up to play any random animation put in configs
		- By default there is a chance at night to play sleep animation if they have use_camp enabled on their walker scheme and using sit animations.
		- Various features in configs\ai_tweaks\move_mgr

	- xr_combat_ignore:
		- Created a flag system when NPC enter/leave no combat smarts. While flagged Stalkers will not be attacked by other stalkers.
			- This fixes the odd behavior of NPCs fighting on the Skadovsk borders by enter/exiting the zone.
			- Completely replaces the no_assault zone space restrictor checks on NPCs. This much better imo.

		- NPCs don't fight during surge until they get to cover
		- Ignore distance remove. Replaced with memory timer
		- Minor changes that improves stealth gameplay
			- NPCs use a search timer when losing sight of enemy
			- Sound/Visual will reset this timer. It's possible to hide someplace quietly for a minute or two to lose enemies.
			- When timer is up they will go about their business

		- Traders use a blank enemy evaluator
		- Monsters and Zombied use vanilla enemy evaluator but with increased ignore distance -- OR NPCs fighting Zombied will use vanilla. This is because of a strange sound detection bug zombied give off.
		- Various features in configs\ai_tweaks\xr_combat_ignore

	- xr_corpse_detection:
		- Prevent detection during surges
		- Prevent looting when commander is in combat
		- Replaced pairs() iteration of the corpse table with a for loop to help free cpu time
		- Changed the way corpses and loot are detected. It is much more CPU efficient.
			- Basically vanilla did this:
				- Iterated through a table containing all the corpses using the slow pairs function.
				- Iterated through ENTIRE inventory of each corpse matching items against the loot table. Yes, Even after it found a match it kept iterating.
				- Selected corpse and executed action
			- My way
				- Iterate through corpse table using a simple much faster for loop
				- Do not iterate at all through NPC inventory, simply raise a flag when items matching the loot table are added to a corpse after it dies
				- When NPC loots all, the flag is removed
				- When Actor loots item, it iterates through NPCs inventory ONLY till it finds a match in the loot table. Sets flag accordingly.

		- NPCs are more likely to loot a corpse and even after something is placed into a dead body. In vanilla sometimes, I don't know why, they didn't.

	- xr_danger:
		- Trigger danger mode via script (Credit: Rulix)
		- NPCs remember a certain danger object as to not run is_danger continously
		- Stalkers now properly exit danger mode.
		- Enabled sound detection for danger.
		- Various features can be altered in configs\ai_tweaks\xr_danger

	- xr_gather_items:
		- Prevent item gathering during surges
		- Custom gather item scheme implemented that can be disabled
		- Allows for artifact detection. As long as NPC seen the artifact (Not a vision check but actually in it's visual memory).
		- NPC must have a detector to pick up artifacts.
		- NPCs use the xr_corpse_detection loot table by default
		- Added gather_artefact_items_enabled to be used in logic just like gather_items_enabled

	- xr_hear:
		- Called scripted danger mode when hearing bullet whizzes or ricochets (Credit: Rulix)

	- xr_help_wounded:
		- Rewritten to fix instances of where some NPC won't get helped.
		- It's no longer a check against memory. It iterates through a table of all online stalkers
		- NPC help wounded squad members first.
		- New feature exported to configs\ai_tweaks\xr_help_wounded
			- When help_in_combat is enabled NPCs will try to help allies during combat
				- This can only occur if Victim is not visible to the helper's enemy (ie. behind some cover)
				- If the distance is too great the helper will only help the victim if he is not visible to his enemy.
			- The above feature can be applied to certain visuals. Leading to future modders adding combat medics with own visuals.

	- xr_reach_task:
		- Npcs SPRINT to cover during a surge
		- NPCs walk with weapon strapped when walking to assigned_targets and when not controlled by other schemes.

	- post_combat_idle:
		- Lowered idle time to 1-7 seconds.

	- release_body_manager:
		- Corpses no longer use a stay time but are released after so many corpses exist. (Credit: Rulix)

	- sim_squad_actions:
		- Squad ST stay time is now dependent on squad behavior id (player_id)
		- Various features can be altered in configs\ai_tweaks\sim_squad_actions

	- sim_squad_scripted:
		- Friendly NPCs will come to actor's aid

	- sim_board:
		- Added callback for squad_on_npc_creation to ai_tweaks.script for use with xrs_rnd_npc_loadout.script

	- game_relations:
		- Fixed an error in set_squad_relation that will fix a couple quests where npcs are suppose to turn friend to actor instead of neutral (ie. Jup_b4_monolit_logic_2)

	- xr_conditions:
		- added dist_to_job_point_ge and dist_to_job_point_le (Mnn)

	- gulag_general:
		- NPCs who have surge jobs run while entering smart_terrain

	- xr_animpoint:
		- Added parse_condlist for reach_movement (Thanks to Mnn)

	- xr_animpoint_predicates:
		- Removed the restriction for visually mask-free only being able to use eat/drink animations when using animpoints.

	- bind_crow:
		- Fixed excessive crow spawning

	- sr_light:
		- Added ability to adjust when NPCs turn on their lamps via their player_id/squad behavior id.
		- These values can be altered in configs\ai_tweaks\sr_light.ltx

	- sr_no_weapon:
		- Enables use of weapon in no_weapon zones.

	- se_stalker:
		- Added callbacks for switch_online and switch_offline for ai_tweaks.script

	- xrs_kill_wounded:
		- Custom kill wounded enemy scheme that overrides vanilla. Hardcoded vanilla version is still there. You  will see them briefly pull pistols out still. Nothing I can do about it.
			- Various tweaking options exist in configs\ai_tweaks\xrs_kill_wounded
				- one option allows a community to cause victims to use the prisoner animation before they are executed.

			- To disable, just removed xrs_kill_wounded.script
		- If enabled and player has Xray Extensions the actor can cause the surrender/prisoner animation to be played on wounded victims.
			- Can be tweaked in configs\ai_tweaks\xr_kill_wounded

	- xrs_facer:
		- A modified version of Rulix's facer scheme from Alundaio's Plugin Pack. (Credit: Rulix)
		- If you have APP use one or the other. Simply delete the file.

	- xr_s:
		- Added a callback to ai_tweaks.script

	- state_mgr_weapon:
		- Added a callback to ai_tweaks.script

	- xrs_rnd_npc_loadout:
		- Added Random Loadout script from Alundaio's Plugin Pack to AI Tweaks 2.03+
		- Gives the ability to create different weapon loadouts for NPCs instead of using clunky xml files.

	- xr_eat_medkit:
		- NPCs have the ability to eat medkits or bandages configuarable in configs\ai_tweaks\xr_eat_medkit (Credit: Rulix)

	- rx_ff:
		- Implemented Rulix's friendly fire script.	(Credit: Rulix)

	- rx_gl:
		- Implemented Rulix's grenade launcher script.	(Credit: Rulix)

	- xr_visual:
		- Implemented a way for NPCs to update visual from armor they acquire. (Credit: ARS Team)
		- Can be tweaked in configs\ai_tweaks\xr_visual

*Config Changes*
	- defines:
		- Increased weapon probability at all rank levels by 10.
		- Increased monster "feel" values for better monster reaction

	- m_stalker:
		- stalker_step_manager
			- Attempt to Fix animation issue with stalkers "sliding" when hit while using run animations.

		- free_stand_run_forward
			- Increased from 3.0 to 4.0

	- Options Pack
		- Various tweaks to m_stalker to make them better in combat, especially monsters.
		- Changed GG points in all.spawn so NPCs can travel between maps.
		- MSO-lite includes all the configuration changes to NPC AI and new added Ecologists.
			- IF you don't use this, you have NO IDEA WHAT YOU ARE MISSING!

*Version Changes*
	- 2.040
		- Changed values in defines.ltx
			- Increased weapon probability at all rank levels by 10.
			- Increased monster "feel" values for better monster reaction

		- Added a modified Look Into Opitical Scope from Rulix's AI Additions
			- Instead of giving this bonus when they are using danger animations I give the bonus when they point the weapon.

		- Files Modified:
			- ai_tweaks.script
			- alun_utils.script
			- defines.ltx
			- xr_gather_items.script
			- xr_visual.script
			- xr_visual.ltx

	- 2.030
		- Added ability to enable ammo usage by NPC weapon in configs\ai_tweaks\ai_tweaks.ltx
		- Added xrs_rnd_npc_loadout.script
		- Added Rulix's rx_gl and rx_ff.
		- Added xr_eat_medkit from a converted version of axr_eat_medkit from APP.
		- Files Modified:
			- system.ltx
			- xr_combat_ignore.script
			- ai_tweaks.script
			- state_mgr_weapon.script
			- rx_ff.script
			- rx_gl.script
			- xr_eat_medkit.script
			- xr_danger.script
			- xrs_rnd_npc_loadout.script
			- alun_utils.script
			- meshes folder
			- textures folder

	- 2.020
		- Fixed xrs_facer.script not reading from ini due to not recognizing actor as enemy
		- Fixed medkit_script not being listed in attachables for m_stalker.ltx (Used for help_wounded)
		- Improved No Weapon Zone behavior.
		- Stalkers will always attack mutants unless specified combat_ignore in logic even in ignored smarts.
		- Added sleep hours for move_mgr's sleep animation

		- Files Modified:
			- xr_combat_ignore.script
			- xrs_facer.script
			- move_mgr.script
			- ai_tweaks.script
			- alun_utils.script

	- 2.010
		- Fixed a bug where Monsters would become nil when dying.
		- Cleaned up backup files from MSO-lite
		- Added the item.ltx back in. Was accidentally left out.
		- Exported script changes in a majority of the scripts into ai_tweaks.script
		- REMOVED modules.script
		- Implemented a way for stalkers to eat/drink while using walker scheme
			- See: move_mgr.script
		- Implemented a new xr_gather_items scheme.
			- See: xr_gather_items.script
		- Fixed a C Stack error
		- Re-fixed Bloodsucker lair quest. Worked 100% of the time in all my tests at 300 switch distance.
		- Various changes to xr_combat_ignore and the my newly added safe_zone flag system
		- Added hang time fixes to xr_help_wounded and xr_corpse_detection
		- Fixed Zombied sprinting during surges
		- Implemented a change to sr_light that prevents the use of light if any stalker using the "sleep" state.
		- Added xrs_facer.script (Rulix's modified Facer scheme from APP)

	- 2.000
		- Jumped version to 2.00 as to avoid any confusion with my old and discontinued project Various AI.
		- Minor bug fixes (ie. sr_light, xr_combat_ignore 1.056)
		- Added config functionality to most script changes [xr_danger,xr_combat_ignore,sim_squad_actions]
		- Added condlist ability to certain aspects of scripts. Danger ignore distance, combat run, search time, etc.
		- Above changes to xr_animpoint_predicates
		- Above changes to sim_squad_actions
		- Increased help wounded distance
		- Changed help wounded animation + allowed it's use in combat under certain conditions
		- In xr_reach_task I made NPCs walk with weapon strapped. Fixed them sprinting with weapon out during surge.
		- Created new scheme xrs_kill_wounded to override the hardcoded vanilla scheme.

	- 1.056
		- Fixed NPCs not reacting to actor during surge.

	- 1.052
		- Fixed NPCs not reacting to monsters and actor during surge.
		- Option Pack for Level-to-level traveling.

*Special Thanks*
	All the hamsters in the world.
	Mnn for advice, scripting queries and his fixes for Kovalski cutscene, labx8 burers and additions to xr_animpoint.
	xStreme, awesome russian scripter.
	Rulix for his AI Additions which I ripped, altered and borrowed.
	jketiynu a.k.a. Swartz for testing, feedback, m_stalker.ltx tweaks, and his alcholism.
	Trojanuch for suggestions, MSO, testing and feedback.
	Ataru Moroboshi

*Credits*
	Rulix AI Additions and utility functions
	ARS Mod Team

