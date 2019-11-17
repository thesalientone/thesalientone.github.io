---
layout: python
title:  "Python Fantasy"
date:   2019-11-17
categories: [coding, personal]
repl: https://the-hard-way.stefiroth.repl.run
excerpt_separator: <!--more-->
---

A couple of years ago I began working on a project that I call PythonFantay. A text based game inspired by Final Fantasy. The game is by no means complete, but I figured I could upload a sample of the game here! The game contains the follow features:

* Save files
* Random Battles
* Party System
* System Menus
* Towns and Scenes

I finished most of the engine/mechanics. The basis of the engine was inspired by : [Learn Python the Hard Way by Zed Shaw](https://learncodethehardway.org)

One day I want to finish the game by adding many more features. Maybe during my next break.. I shall see! Until then please enjoy!

Even though it's not written, you can exit from most menus by typing "exit". Good luck!



{% highlight python %}

import pickle
import math
from random import randint, random


class Engine(object):
    def __init__(self, scene_map):
        self.scene_map = scene_map

    def play(self):
        current_scene = self.scene_map.opening_scene()
        last_scene = self.scene_map.next_scene('finished')
        prev_scene = self.scene_map.name_of_scene(current_scene)

        next_scene_name = current_scene.enter()
        current_scene = self.scene_map.next_scene(next_scene_name)
        leaving_menu = 0
        outcome = None

        while current_scene != last_scene:

            if next_scene_name in [
                    'menu', 'inv', 'save', 'equip', 'load', 'mhelp', 'debug',
                    'magic'
            ]:
                leaving_menu = 1
                if next_scene_name != 'menu':

                    next_scene_name = 'menu'

                else:

                    next_scene_name = current_scene.enter(prev_scene)
                    current_scene = self.scene_map.next_scene(next_scene_name)

            elif next_scene_name in ['help']:
                next_scene_name = current_scene.enter(prev_scene)
                current_scene = self.scene_map.next_scene(next_scene_name)

            else:
                logic1 = 0
                logic2 = 0
                if prev_scene not in self.scene_map.towns:
                    if prev_scene not in ['open', 'start', 'menu']:
                        logic1 = 1
                if next_scene_name not in self.scene_map.towns:
                    if next_scene_name not in ['open', 'start', 'menu']:
                        logic2 = 1
                if logic1 * logic2 == 1:
                    #print "P scene %s N  scene %s" % (prev_scene, next_scene_name)
                    #print self.scene_map.towns
                    if randint(0, 100) <= 35 and leaving_menu != 1:
                        outcome = Battle().enter(prev_scene)

                if outcome:

                    current_scene = self.scene_map.next_scene(outcome)
                    outcome = None
                leaving_menu = 0
                next_scene_name = current_scene.enter()
                prev_scene = self.scene_map.name_of_scene(current_scene)
                current_scene = self.scene_map.next_scene(next_scene_name)

        current_scene.enter()


class Scene(object):
    def enter(self):
        print "scene not yet finished"

    def game_over_scene(self, hploss):
        hp_sum = 0
        PARTY.party_subract_hp(hploss)
        for hero in PARTY.get_party_members():
            hp_sum += hero.hp

        return hp_sum

    def restore_party(self):
        for hero in PARTY.get_party_members():
            hero.hp = hero.maxhp
            hero.mp = hero.maxmp
        battleborders("Party HP and MP Restored")


#Battle is a type of Scene


class Party(object):

    intro_text = """

Welcome. Choose a name and class for 4 heroes.

The following classes are available:

1. Warrior
2. White Mage
3. Black Mage
4. Theif

    """

    def __init__(self):

        formatborders(self.intro_text)
        self.hero1 = eval(self.change_job(self.get_job()))
        self.hero2 = eval(self.change_job(self.get_job()))
        self.hero3 = eval(self.change_job(self.get_job()))
        self.hero4 = eval(self.change_job(self.get_job()))
        self.inv = Inventory()
        self.initialize_inventory()
        self.gil = 50

    def get_job(self):

        job = raw_input("Enter the job of your hero:\n> ")

        return job

    def change_job(self, info):

        jobs = {
            'warrior': "Warrior()",
            'Warrior': "Warrior()",
            '1': "Warrior()",
            'white mage': "WhiteMage()",
            'White Mage': "WhiteMage()",
            'white': "WhiteMage()",
            '2': "WhiteMage()",
            'black mage': "BlackMage()",
            'Black Mage': "BlackMage()",
            'black': "BlackMage()",
            '3': "BlackMage()",
            'theif': "Theif()",
            'Theif': "Theif()",
            '4': "Theif()"
        }

        while info not in jobs.keys():
            print "You have not selected a proper job."
            formatborders(self.intro_text)
            info = self.get_job()
        return jobs[info]

    def get_party_members(self):
        party_members = [self.hero1, self.hero2, self.hero3, self.hero4]

        return party_members

    def initialize_inventory(self):
        self.inv.add_item(potion, 10)
        self.inv.add_item(ether, 5)
        self.inv.add_item(hipotion, 1)
        self.inv.add_item(hiether, 1)

        for hero in self.get_party_members():
            hero.initialize_equipment()

    def display_equipment(self):

        formatborders(
            "EQUIPMENT: Equipment Stats are in the format HP/MP/ATT/MATT/PARM/MARM"
        )
        party_members = self.get_party_members()
        print "NAME".ljust(7),
        for hero in party_members:
            print hero.name.ljust(15),
        print "\n", "JOB".ljust(7),
        for hero in party_members:
            print hero.job.ljust(15),
        print "\n", "WEAPON".ljust(7),
        for hero in party_members:
            print hero.sword.name.ljust(15),
        print "\n"
        print "\n", "".ljust(7),
        for hero in party_members:
            hero.sword.print_stats(),
        print "\n"
        print "\n", "HELM".ljust(7),
        for hero in party_members:
            print hero.helm.name.ljust(15),
        print "\n"
        print "\n", "".ljust(7),
        for hero in party_members:
            hero.helm.print_stats(),
        print "\n"
        print "\n", "CHEST".ljust(7),
        for hero in party_members:
            print hero.chest.name.ljust(15),
        print "\n"
        print "\n", "".ljust(7),
        for hero in party_members:
            hero.chest.print_stats(),
        print "\n"
        print "\n", "ACC".ljust(7),
        for hero in party_members:
            if hero.acc:
                print hero.acc.ljust(15),
            else:
                print 'NONE'.ljust(15),
        print "\n"

    def display_heros(self):
        print "\n"
        members = self.get_party_members()
        for i in range(4):
            class_name = members[i].__class__.__name__
            print i + 1, ". %s" % members[i].name.ljust(
                10), "(%s)" % class_name
        print "\n"

    def return_avg_levels(self):
        lvlavg = self.hero1.lev + self.hero2.lev + self.hero3.lev + self.hero4.lev
        lvlavg /= 4
        return lvlavg

    def display_hp(self):
        display_format = "{:<10}" + ("{:^10}" * 4)

        hero_hp = ["HP:"]
        heros = self.get_party_members()
        for hero in heros:
            hero_hp.append("%d/%d" % (hero.hp, hero.maxhp))

        print display_format.format(*hero_hp)

    def display_hero_names(self):
        display_format = "{:<10}" + ("{:^10}" * 4)
        hero_names = ['Name:']
        for hero in self.get_party_members():
            hero_names.append(hero.name)
        print display_format.format(*hero_names)

    def display_mp(self):

        display_format = "{:<10}" + ("{:^10}" * 4)

        hero_mp = ["MP:"]
        heros = self.get_party_members()
        for hero in heros:
            hero_mp.append("%d/%d" % (hero.mp, hero.maxmp))

        print display_format.format(*hero_mp)

    def display_stat(self, stat):

        if stat == 'hp':
            self.display_hp()
        elif stat == 'mp':
            self.display_mp()
        else:
            display_format = "{:<10}" + ("{:^10}" * 4)
            text = [stat.upper()]
            for hero in self.get_party_members():
                evalcode = 'hero.' + stat
                text.append(eval(evalcode))
            print display_format.format(*text)
            if stat == 'name':
                print "-" * 50

    def display_panel(self):
        print "-" * 80
        self.display_hero_names()
        self.display_hp()
        self.display_mp()
        print "-" * 80

    def display_panel_numbered(self):
        print "-" * 80
        self.display_hero_names_numbered()
        self.display_hp()
        self.display_mp()
        print "-" * 80

    def display_full_panel(self):
        stat_names = [
            'name', 'job', 'lev', 'hp', 'mp', 'exp', 'mag', 'mdef', 'stre',
            'defe', 'speed'
        ]
        for stat_name in stat_names:
            self.display_stat(stat_name)

    def display_hero_names_numbered(self):
        display_format = "{:<10}" + "{:^10}" * 4
        heros = self.get_party_members()
        display_text = ['NAME:']
        for i in range(4):
            name_text = "%d. %s" % (i + 1, heros[i].name)
            display_text.append(name_text)
        print display_format.format(*display_text)

    def party_subract_hp(self, health):
        for hero in self.get_party_members():
            hero.hp -= health
            if hero.hp < 0:
                hero.hp = 0


class Equipment(object):

    equip_stat_string_list = [
        'att', 'matt', 'parm', 'marm', 'health', 'mana', 'cost'
    ]

    def __init__(self, name, att, matt, parm, marm, health, mana, cost,
                 armortype):
        self.name = name
        self.att = att
        self.matt = matt
        self.parm = parm
        self.marm = marm
        self.health = health
        self.mana = mana
        self.cost = cost
        self.armortype = armortype

    def return_stats(self):
        stat_list = [
            self.health, self.mana, self.att, self.matt, self.parm, self.marm
        ]
        return stat_list

    def print_stats(self):
        stat_text = []
        for stat in self.return_stats():
            stat_text.append(str(stat))
        print "/".join(stat_text).ljust(15),

    def print_stats_inventory(self):
        print "NAME".ljust(10), self.name
        for stat in self.equip_stat_string_list:

            run_code = 'self.' + stat
            print stat.upper().ljust(10), eval(run_code)


class Accesory(Equipment):

    pass


class Item(object):
    def __init__(self, name, hp, mp, desc, itype):
        self.name = name
        self.hp = hp
        self.mp = mp
        self.desc = desc
        self.itype = itype


#Equipment Declarations name, att, matt, parm, marm, health, cost, armortype
bronzehelm = Equipment('Bronze Helm', 1, 0, 5, 3, 10, 0, 100, 'whelm')
bronzechestplate = Equipment('Bronze Plate', 1, 0, 10, 5, 0, 0, 100, 'wchest')
bronzesword = Equipment('Bronze Sword', 10, 0, 0, 0, 0, 0, 100, 'wwep')
leatherhelm = Equipment('Leather Helm', 0, 1, 5, 5, 5, 10, 100, 'mhelm')
leatherchestplate = Equipment('Leather Plate', 0, 1, 7, 11, 3, 0, 100,
                              'mchest')
woodenwand = Equipment('Wooden Wand', 3, 10, 0, 2, 0, 5, 100, 'mwep')
ironhelm = Equipment('Iron Helm', 1, 0, 15, 7, 0, 0, 150, 'whelm')
ironchest = Equipment('Iron Chest', 0, 0, 15, 7, 0, 0, 200, 'wchest')
potion = Item('Potion', 100, 0, 'Restores 100 HP', 'p')
ether = Item('Ether', 0, 20, 'Restores 200 MP', 'm')
phoenixdown = Item('Phoenix Down', 10000, 0, 'Revives dead Hero', 'p')
hipotion = Item('Hi-Potion', 500, 0, 'Restores 500 HP', 'p')
hiether = Item('Hi-Ether', 0, 500, 'Restores 200 MP', 'm')


class Hero(object):

    action_text = """
    Select an action: (Press enter to skip turn)
    1. Attack
    2. Magic
    3. Use item
    4. Flee
>"""

    def __init__(self):

        self.lev = 0
        self.exp = 0
        self.mag = 0
        self.mdef = 0
        self.stre = 0
        self.defe = 0
        self.maxhp = 0
        self.hp = 0
        self.maxmp = 0
        self.mp = 0
        self.name = None
        self.speed = 0
        self.job = self.__class__.__name__
        self.initialize()
        self.stats = {
            'name': self.name,
            'job': self.job,
            'lev': self.lev,
            'exp': self.exp,
            'mag': self.mag,
            'mdef': self.mdef,
            'stre': self.stre,
            'defe': self.defe,
            'hp': self.hp,
            'mp': self.mp,
            'speed': self.speed
        }
        self.sword = None
        self.helm = None
        self.chest = None
        self.acc = None
        self.spells = []

    def initialize(self):

        self.name = raw_input("Enter a hero name:\n> ")
        if len(self.name) > 11 or len(self.name) < 1:
            print "Please Enter a hero name that is 10 characters or less."
            self.initialize()

    def return_stats(self):
        hero_stats = [
            self.name, self.lev, self.exp, self.mag, self.mdef, self.stre,
            self.defe, self.hp, self.mp, self.speed, self.maxhp, self.maxmp
        ]
        return hero_stats

    def return_some_stats(self):
        pass

    def equip(self, equipment, eqtype):
        if eqtype == 'sword':
            self.sword = equipment
        if eqtype == 'helm':
            self.helm = equipment
        if eqtype == 'chest':
            self.chest = equipment
        if eqtype == 'acc':
            self.acc = equipment

    def set_stats(self, lev, exp, mag, mdef, stre, defe, hp, maxhp, mp, maxmp,
                  speed):
        self.lev = lev
        self.exp = exp
        self.mag = mag
        self.mdef = mdef
        self.stre = stre
        self.defe = defe
        self.hp = hp
        self.mp = mp
        self.speed = speed
        self.maxhp = maxhp
        self.maxmp = maxmp

    def act(self, target_list):
        print "%s's turn:" % self.name
        action = raw_input(self.action_text)
        if action in ['1', 'attack', 'Attack', 'a', 'A']:
            return self.attack(target_list)
        elif action in ['2', 'magic', 'Magic', 'm', 'M']:
            return self.magic(target_list)
        elif action in ['3', 'item', 'i', 'I', 'Item']:
            InventoryMenu().enter('battle')
            return 0, 0
        elif not action:
            print "Skipping Turn"
            return 0, 0
        else:
            print "Please enter an action. Press enter to skip the turn"
            return self.act(target_list)

    def attack(self, target_list):
        self.printtargets(target_list)
        target = raw_input(">")
        if target.isdigit():
            target = int(target)
            #print range(1, len(target_list[4:]) + 1)
            if target not in range(1, len(target_list[4:]) + 1):
                print "Target out of range."
                return self.attack(target_list)
            monster = target_list[3 + target].__class__.__name__
            damage = self.generate_attack_dmg(target_list[3 + target])
            battleborders("%s attacked %s doing %d damage." %
                          (self.name, monster, damage))

            return damage, 3 + target
        else:
            print "Please enter a number."
            return self.attack(target_list)

    def printtargets(self, target_list):
        index = 1
        print "Enter a target number:"
        for target in range(4, len(target_list[4:]) + 4):
            monster = target_list[target]
            print index, ". ", monster.__class__.__name__, " ",
            index += 1
        print ""

    def generate_attack_dmg(self, monster):
        delta = self.sword.att + self.helm.att + self.chest.att
        D = (self.stre / 2) + delta
        damage = D + D * random() - monster.defe
        return math.ceil(damage)
        #return (random() + randint (5, 10)) * ((-1)  ** 2)

    def print_levelup(self):
        battleborders("%s has reached level %d" % (self.name, self.lev))

    def display_levelup_stat(self, stat_txt, old, new):
        display_format = "{:<10}" + "{:^+10}"
        display_text = [stat_txt.upper(), new - old]
        print display_format.format(*display_text)

    def display_levelup_stats(self, old_stats, new_stats):
        stat_names = ['mag', 'mdef', 'stre', 'defe', 'speed', 'maxhp', 'maxmp']
        for i in range(7):
            self.display_levelup_stat(stat_names[i], old_stats[i],
                                      new_stats[i])

    def display_spells(self):
        print "%s Spells: " % (self.name)
        print '-' * 80
        spell_count = len(self.spells)
        y = enumerate(self.spells, 1)
        spells_numbered = list(y)

        spell_text = []
        for spell in spells_numbered:
            spell_text.append("%d. %s" % (spell[0], spell[1].name.upper()))
        extend_amount = (4 - ((len(self.spells) + 4) % 4))

        for x in range(extend_amount):
            spell_text.append(" ")

        i = 0
        display_text = "{:>10}" * 4
        while i <= spell_count:
            #print "Spell %r is number %d" % (spell_text[i], i)
            print display_text.format(*spell_text[i:i + 4])

            i += 4
        print '-' * 80

    def generate_magic_dmg(self, spell, monster):

        delta = self.sword.matt + self.helm.matt + self.chest.matt
        D = (self.mag / 2) + delta
        self.mp -= spell.mp
        damage = (D + D * random() - monster.mdef) * spell.potency
        return math.ceil(damage)

    def generate_magic_healing(self, spell):
        delta = self.sword.matt + self.helm.matt + self.chest.matt
        D = (self.mag / 2) + delta
        damage = spell.potency * (D + D * random())
        return math.ceil(damage)

    def use_spell(self, spell, target):
        if target.__class__ not in Hero_Classes:
            pass  # Battle use of spell on monster
        else:
            if self.mp >= spell.mp:
                beforehp = target.hp
                healing_amount = self.generate_magic_healing(spell)
                target.hp += healing_amount
                self.mp -= spell.mp
                if target.hp > target.maxhp:
                    target.hp = target.maxhp
                print "Restored %d hp to %s" % (taget.hp - beforehp,
                                                target.name)

    def magic(self, target_list):
        spell = self.get_spell()
        if spell:

            hero_list = PARTY.get_party_members()
            if spell.foe_target == 'friend':
                PARTY.display_panel_numbered()
                target_choice = raw_input("\n>")
                if (int(target_choice) <= 4) and (int(target_choice) >= 1):
                    hero.use_spell(spell, hero_list[int(target_choice) - 1])
                    return 0, 0
                else:
                    print "Please select a hero numbered 1 through 4"
                    return self.act(target_list)
            else:
                self.printtargets(target_list)
                target = raw_input(">")
                if target.isdigit():
                    target = int(target)
                    #print range(1, len(target_list[4:]) + 1)
                    if target not in range(1, len(target_list[4:]) + 1):
                        print "Target out of range."
                        return self.attack(target_list)
                    monster = target_list[3 + target].__class__.__name__

                    if self.mp > spell.mp:
                        damage = self.generate_magic_dmg(
                            spell, target_list[3 + target])
                        battleborders("%s blasted %s doing %d damage." %
                                      (self.name, monster, damage))

                        return damage, 3 + target
                    else:
                        print "Not enough mp. "
                        return 0, 0
                else:
                    print "Please enter a number."
                    return self.act(target_list)
        else:
            return self.act(target_list)

    def get_spell(self):
        if len(self.spells) == 0:
            print "Hero has no spells. "
            return None
        else:
            self.display_spells()
            print "Select spell number"
            spell_choice = raw_input("\n>")
            if spell_choice.isdigit():
                spell_choice = int(spell_choice)
                return self.spells[spell_choice - 1]

            else:
                print "Enter a proper spell number."
                return get_spell()


class Warrior(Hero):
    def level_up(self, leftover_exp):
        oldstats = self.return_stats()
        oldstats = oldstats[3:]
        oldstats.pop(4)
        oldstats.pop(4)
        self.lev += 1
        self.exp = int(100 * (1.1**self.lev) - leftover_exp)
        self.mag += 1
        self.mdef += 2
        self.stre += 5
        self.defe += 4
        self.maxhp += 100 + randint(0, 50)
        self.maxmp += 10
        self.speed += 1
        self.hp = self.maxhp
        self.mp = self.maxmp
        self.time = 100 / self.speed
        newstats = self.return_stats()
        newstats = newstats[3:]
        newstats.pop(4)
        newstats.pop(4)
        self.print_levelup()
        self.display_levelup_stats(oldstats, newstats)

    def initialize_equipment(self):
        self.equip(bronzesword, 'sword')
        self.equip(bronzehelm, 'helm')
        self.equip(bronzechestplate, 'chest')
        self.set_stats(1, 100, 5, 6, 9, 8, 100, 100, 5, 5, 10)
        self.time = 100 / self.speed


class WhiteMage(Hero):
    def level_up(self, leftover_exp):
        oldstats = self.return_stats()
        oldstats = oldstats[3:]
        oldstats.pop(4)
        oldstats.pop(4)
        self.lev += 1
        self.exp = int(100 * (1.1**self.lev) - leftover_exp)
        self.mag += 4
        self.mdef += 6
        self.stre += 1
        self.defe += 1
        self.maxhp += 20 + randint(0, 30)
        self.maxmp += 10 + randint(0, 5)
        self.hp = self.maxhp
        self.mp = self.maxmp
        self.speed += 1.5
        self.time = 100 / self.speed
        newstats = self.return_stats()
        newstats = newstats[3:]
        newstats.pop(4)
        newstats.pop(4)
        self.print_levelup()
        self.display_levelup_stats(oldstats, newstats)

    def initialize_equipment(self):
        self.equip(woodenwand, 'sword')
        self.equip(leatherhelm, 'helm')
        self.equip(leatherchestplate, 'chest')
        self.set_stats(1, 100, 10, 7, 1, 3, 60, 60, 60, 60, 10)
        self.time = 100 / self.speed
        self.spells.append(cure)


class BlackMage(Hero):
    def level_up(self, leftover_exp):
        oldstats = self.return_stats()
        oldstats = oldstats[3:]
        oldstats.pop(4)
        oldstats.pop(4)
        self.lev += 1
        self.exp = int(100 * (1.1**self.lev) - leftover_exp)
        self.mag += 7
        self.mdef += 4
        self.stre += 2
        self.defe += 1
        self.maxhp += 20 + randint(0, 30)
        self.maxmp += 10 + randint(0, 5)
        self.hp = self.maxhp
        self.mp = self.maxmp
        self.speed += 1.5
        self.time = 100 / self.speed
        newstats = self.return_stats()
        newstats = newstats[3:]
        newstats.pop(4)
        newstats.pop(4)
        self.print_levelup()
        self.display_levelup_stats(oldstats, newstats)

    def initialize_equipment(self):
        self.equip(woodenwand, 'sword')
        self.equip(leatherhelm, 'helm')
        self.equip(leatherchestplate, 'chest')
        self.set_stats(1, 100, 10, 7, 1, 3, 60, 60, 60, 60, 10)
        self.time = 100 / self.speed
        self.spells.append(fire)


class Theif(Hero):
    def level_up(self, leftover_exp):
        oldstats = self.return_stats()
        oldstats = oldstats[3:]
        oldstats.pop(4)
        oldstats.pop(4)
        self.lev += 1
        self.exp = int(100 * (1.1**self.lev) - leftover_exp)
        self.mag += 1
        self.mdef += 2
        self.stre += 6
        self.defe += 2
        self.maxhp += 60 + randint(0, 30)
        self.maxmp += 10
        self.hp = self.maxhp
        self.mp = self.maxmp
        self.speed += 2
        self.time = 100 / self.speed
        newstats = self.return_stats()
        newstats = newstats[3:]
        newstats.pop(4)
        newstats.pop(4)
        self.print_levelup()
        self.display_levelup_stats(oldstats, newstats)

    def initialize_equipment(self):
        self.equip(bronzesword, 'sword')
        self.equip(bronzehelm, 'helm')
        self.equip(bronzechestplate, 'chest')
        self.set_stats(1, 100, 5, 6, 7, 8, 80, 80, 5, 5, 12)
        self.time = 100 / self.speed


class Action(object):
    def attack(self, target_list):
        self.printtargets(target_list)
        target = raw_input(">")
        if target.isdigit():
            target = int(target)
            #print range(1, len(target_list[4:]) + 1)
            if target not in range(1, len(target_list[4:]) + 1):
                print "Target out of range."
                return self.attack(target_list)
            monster = target_list[3 + target].__class__.__name__
            damage = self.generate_attack_dmg(target_list[3 + target])
            battleborders("%s attacked %s doing %d damage." %
                          (self.name, monster, damage))

            return damage, 3 + target
        else:
            print "Please enter a number."
            return self.attack(target_list)

    def printtargets(self, target_list):
        index = 1
        print "Enter a target number:"
        for target in range(4, len(target_list[4:]) + 4):
            monster = target_list[target]
            print index, ". ", monster.__class__.__name__, " ",
            index += 1
        print ""

    def generate_attack_dmg(self, monster):
        delta = self.sword.att + self.helm.att + self.chest.att
        D = (self.stre / 2) + delta
        damage = D + D * random() - monster.defe
        return math.ceil(damage)
        #return (random() + randint (5, 10)) * ((-1)  ** 2)

    def print_levelup(self):
        battleborders("%s has reached level %d" % (self.name, self.lev))

    def display_levelup_stat(self, stat_txt, old, new):
        display_format = "{:<10}" + "{:^+10}"
        display_text = [stat_txt.upper(), new - old]
        print display_format.format(*display_text)

    def display_levelup_stats(self, old_stats, new_stats):
        stat_names = ['mag', 'mdef', 'stre', 'defe', 'speed', 'maxhp', 'maxmp']
        for i in range(7):
            self.display_levelup_stat(stat_names[i], old_stats[i],
                                      new_stats[i])


class Spell(Action):
    def __init__(self, **kwargs):
        allowed_keys = [
            'name', 'foe_target', 'multi_target', 'damage_type', 'potency',
            'desc', 'mp'
        ]
        self.__dict__.update(
            (k, v) for k, v in kwargs.iteritems() if k in allowed_keys)


cure = Spell(
    name='Cure',
    foe_target='friend',
    damage_type='magical',
    potency=10,
    desc="Restores a small amount of HP",
    mp=5)
fire = Spell(
    name='Fire',
    foe_target='foe',
    damage_type='magical',
    potency=10,
    desc="Deals a small amount of fire damage.",
    mp=5)


class Monster(object):
    action_text = """
    Select an action:
    1. Attack
>"""

    itemdrops = [potion]
    equipdrops = [bronzesword]

    def __init__(self, lev):

        self.lev = lev
        self.exp = 0
        self.mag = 0
        self.mdef = 0
        self.stre = 0
        self.defe = 0
        self.maxhp = 0
        self.hp = 0
        self.maxmp = 0
        self.mp = 0

        self.speed = 0
        self.job = self.__class__.__name__

        self.stats = {
            'job': self.job,
            'lev': self.lev,
            'exp': self.exp,
            'mag': self.mag,
            'mdef': self.mdef,
            'stre': self.stre,
            'defe': self.defe,
            'hp': self.hp,
            'mp': self.mp,
            'speed': self.speed
        }

    def act(self, target_list):
        print "%s is " % self.__class__.__name__,
        action = randint(1, 1)
        if action == 1:
            return self.attack(target_list)

        return 0, 0

    def attack(self, target_list):

        hero, target = self.find_target(target_list)
        damage = self.generate_attack_dmg(target)
        print "attacking %s for %d" % (target.name, damage)
        print "-" * 80

        return damage, hero

    def printtargets(self, target_list):
        for target in target_list[:3]:
            print target.__class__.__name__

    def generate_attack_dmg(self, hero):
        delta = hero.helm.parm + hero.chest.parm + hero.sword.parm
        damage = .1 * self.stre * (self.stre - delta) * (1 - (random() / 5))
        if damage < 0:
            damage = 0
        return math.ceil(damage)

    def rewards(self):
        loot = []
        if random() <= .3:
            loot.append(self.itemdrops[randint(0, len(self.itemdrops) - 1)])
        if random() <= .1:
            loot.append(self.equipdrops[randint(0, len(self.equipdrops) - 1)])

        return loot

    def find_target(self, target_list):
        found_target = 0
        while found_target == 0:
            hero = randint(0, 3)
            target = target_list[hero]
            if target.hp > 0:
                found_target = 1
        return hero, target


class Slime(Monster):

    itemdrops = [potion]
    equipdrops = [ironchest]

    def __init__(self, lev):
        super(Slime, self).__init__(lev)

        self.adjust_stats()

    def adjust_stats(self):
        #print "Stats"
        self.stre = self.lev + 20
        self.hp = self.lev * 50
        self.maxhp = self.hp
        self.speed = self.lev + 5
        self.time = 100 / (self.speed)
        self.exp = 10 * self.lev


class Inventory(object):

    item_dictionary = {
        'Potion': potion,
        'Ether': ether,
        'Hi-Potion': hipotion,
        'Hi-Ether': hiether,
        'Phoenix Down': phoenixdown,
        'Iron Helm': ironhelm,
        'Iron Chest': ironchest,
        'Bronze Helm': bronzehelm,
        'Bronze Plate': bronzechestplate,
        'Bronze Sword': bronzesword,
        'Leather Helm': leatherhelm,
        'Leather Plate': leatherchestplate,
        'Wooden  Wand': woodenwand
    }

    def __init__(self):
        self.contents = {}

    def add_item(self, item, amount):
        if item.name in self.contents.keys():
            self.contents[item.name] += amount
        else:
            self.contents[item.name] = amount

    def sub_item(self, item, amount):
        if item.name in self.contents.keys():
            self.contents[item.name] -= amount
            if self.contents[item.name] <= 0:
                del self.contents[item.name]

    def select_item(self, item_list):

        item_number = raw_input(
            "Enter the number or of the item you wish to use:")

        try:
            item_to_use = self.item_dictionary[item_list[int(item_number) -
                                                         1][1]]
            class_name = self.item_dictionary[item_list[int(item_number) - 1]
                                              [1]].__class__.__name__
            if class_name == "Item":
                if item_number:
                    print item_list[int(item_number) -
                                    1][1], " Description: \n"
                    print item_to_use.desc

                    return item_number, item_to_use

                else:
                    return None
            elif class_name == "Equipment":
                item_to_use.print_stats_inventory()
                return None
        except:
            print "Improper item number entered."
            formatborders("MENU")
            return None

    def select_equipment(self, item_list):
        item_number = raw_input(
            "Enter the number of the item you wish to equip:")

        #try goes here

        if item_number:
            item_to_use = self.item_dictionary[item_list[int(item_number) -
                                                         1][1]]
            wep_type = item_to_use.armortype
            PARTY.display_heros()
            hero_num = raw_input(
                "Enter the number of the hero to equip the %s to: " %
                item_to_use.name)
            members = PARTY.get_party_members()
            if hero_num:
                hero = members[int(hero_num) - 1]
                if wep_type in ['whelm', 'mhelm']:
                    hero_wep_type = hero.helm.armortype
                    if wep_type == hero_wep_type:
                        formatborders(hero.name)
                        self.compare_equipment(hero.helm, item_to_use)
                        confirm_change = raw_input(
                            "Do you wish to change the equipment (Y/N): \n>")

                        if confirm_change in [
                                'y', 'Y', 'yes', 'Yes', item_to_use.name
                        ]:
                            PARTY.inv.add_item(hero.helm, 1)
                            hero.helm = item_to_use
                            PARTY.inv.sub_item(item_to_use, 1)
                            print "%s successfully equipped. " % item_to_use.name
                    else:
                        print "This equipment is not for that class of hero."
                elif wep_type in ['wchest', 'mchest']:
                    hero_wep_type = hero.chest.armortype
                    if wep_type == hero_wep_type:
                        formatborders(hero.name)
                        self.compare_equipment(hero.chest, item_to_use)
                        confirm_change = raw_input(
                            "Do you wish to change the equipment (Y/N): \n>")

                        if confirm_change in [
                                'y', 'Y', 'yes', 'Yes', item_to_use.name
                        ]:
                            PARTY.inv.add_item(hero.chest, 1)
                            hero.chest = item_to_use
                            PARTY.inv.sub_item(item_to_use, 1)
                            print "%s successfully equipped. " % item_to_use.name
                    else:
                        print "This equipment is not for that class of hero."
                elif wep_type in ['wwep', 'mwep']:
                    hero_wep_type = hero.sword.armortype
                    if wep_type == hero_wep_type:
                        formatborders(hero.name)
                        self.compare_equipment(hero.sword, item_to_use)
                        confirm_change = raw_input(
                            "Do you wish to change the equipment (Y/N): \n>")

                        if confirm_change in [
                                'y', 'Y', 'yes', 'Yes', item_to_use.name
                        ]:
                            PARTY.inv.add_item(hero.sword, 1)
                            hero.sword = item_to_use
                            PARTY.inv.sub_item(item_to_use, 1)
                            print "%s successfully equipped. " % item_to_use.name
                    else:
                        print "This equipment is not for that class of hero."

                else:
                    print "Something went wrong. Don't save. You might ruin your file. "

            else:
                print "No hero selected."
                return None
        else:
            print "No item selected. Returning to menu"
            return None

    def compare_equipment(self, equip1, equip2):

        print "NAME".ljust(10), equip1.name.ljust(15), equip2.name.ljust(
            15), "CHANGE"
        for stat in Equipment.equip_stat_string_list:
            old = eval('equip1.' + stat)
            new = eval('equip2.' + stat)
            diff = new - old
            if diff > 0:
                diff = '+' + str(diff)
            print stat.upper().ljust(10), str(old).ljust(15), str(new).ljust(
                15), diff

    def use_item(self, item):

        members = PARTY.get_party_members()
        PARTY.display_hero_names_numbered()
        PARTY.display_hp()
        PARTY.display_mp()

        hero_number = raw_input(
            "Enter the number of the hero to use the %s on: " % item.name)
        if hero_number:
            try:
                hero = members[int(hero_number) - 1]
                #If using a potion to restore HP
                if item.itype == 'p':
                    hero_temp_stat = hero.hp
                    if hero.hp == 0 and item.name != 'Phoenix Down':
                        print "Error: Must use Phoenix Down or rest at an Inn"
                        hero_temp_stat = 0
                    if hero.hp == 0 and item.name == 'Phoenix Down':
                        self.use_potion(item, hero)
                        self.sub_item(item, 1)
                        hero_temp_stat = hero.hp - hero_temp_stat
                    if hero.hp > 0 and item.name != 'Phoenix Down':
                        self.use_potion(item, hero)
                        self.sub_item(item, 1)
                        hero_temp_stat = hero.hp - hero_temp_stat

                    print "Restored %d health to %s. " % (hero_temp_stat,
                                                          hero.name)
                #If using a potion to restore mp
                if item.itype == 'm':
                    hero_temp_stat = hero.mp
                    self.use_potion(item, hero)
                    self.sub_item(item, 1)
                    hero_temp_stat = hero.mp - hero_temp_stat

                    print "Restored %d mana to %s. " % (hero_temp_stat,
                                                        hero.name)
            except:
                print "Improper hero number entered. Returning to menu."
                formatborders("MENU")
        else:
            print "No hero entered. Returning to menu. "
            formatborders("MENU")

    def use_potion(self, item, hero):

        hero_new_hp = hero.hp + item.hp

        if hero_new_hp >= hero.maxhp:
            hero.hp = hero.maxhp
        else:
            hero.hp = hero_new_hp

        hero_new_mp = hero.mp + item.mp
        if hero_new_mp >= hero.maxmp:
            hero.mp = hero.maxmp
        else:
            hero.mp = hero_new_mp

    def display_items(self, option):
        if option == 'item':
            pack = self.contents
        elif option == 'equip':
            pack = self.seive_equipment()

        else:
            pack = self.contents

        max_number = 20
        number_of_items = len(pack)

        enum = enumerate(pack.keys())
        numbered_list = list(enum)
        if number_of_items <= 20:
            for item in numbered_list:
                print item[0] + 1, ". ", item[1], ": x", pack[item[1]]

        else:
            for row in range(0, max_number):

                for column in range(0, 4):
                    point = row + max_number * column
                    if point < number_of_items:
                        try:
                            print point + 1, numbered_list[point][
                                0] + 1, ". ", numbered_list[point][
                                    1], ": x", pack[numbered_list[point][1]],
                        except:
                            pass

                print ""
        return numbered_list

    def seive_equipment(self):

        equipment_dictionary = {}

        for item_name in self.contents.keys():
            if self.item_dictionary[item_name].__class__.__name__ in [
                    'Equipment', 'Accesory'
            ]:
                equipment_dictionary[item_name] = self.contents[item_name]

        return equipment_dictionary


class Opening(Scene):
    def enter(self):
        self.entrytext = """

    This is the start of a game.

    You can type "exit" (without the quotes) to exit from most menus.

    Press enter 1 for menu
    Press 2 to injure hero1 for testing
    Press 3 to add Iron Helm, Iron Chest for testing
    Enter 4 to go to the field.
    """
        formatborders(self.entrytext)

        prompt = raw_input("What do you want to do: \n>")
        if prompt in ['1', 'menu', 'Menu']:
            formatborders("MENU")

            return 'menu'
        elif prompt in ['2']:
            PARTY.hero1.hp -= 20
            print 'DIE HERO!!! Now his health is %d' % PARTY.hero1.hp
            return 'open'
        elif prompt in ['3']:
            PARTY.inv.add_item(ironhelm, 1)
            PARTY.inv.add_item(ironchest, 1)
            print "Added Iron Helm"
            return 'open'
        elif prompt in ['quit', 'q', 'exit', 'Exit']:
            print "Game Exiting. Thank you for playing."
            return 'finished'
        elif prompt in ['4', 'field', 'Field']:
            print "Going to field."
            return 'field_0_1'

        else:
            return 'open'


class GameStart(Scene):
    global PARTY

    def enter(self):
        global PARTY
        start_text = """

Python Fantasy
by Stefan Jenkins


1. New Game
2. Load Game
3. Help
4. Exit Game

        """
        formatborders(start_text)
        start_option = raw_input("Type a Number or Command:\n>")

        if start_option in ['1', 'new', 'New', 'n', 'N']:
            print "Starting New Game........"
            PARTY = Party()
            return 'open'

        elif start_option in ['2', 'load', 'Load', 'l', 'L']:
            print "Loading Save File........"
            load_data = self.check_memory_card()
            load_data.display_saves()

            load_slot = raw_input("Which file would you like to load: \n>")
            try:
                load_slot = int(load_slot)
            except:
                load_slot = 0

            if load_slot == 1:
                if load_data.save1:
                    PARTY = load_data.save1.party
                    return load_data.save1.scene_name
                else:
                    print "No save file found. Returning to start."
                    return 'start'
            if load_slot == 2:
                if load_data.save2:
                    PARTY = load_data.save2.party
                    return load_data.save2.scene_name
                else:
                    print "No save file found. Returning to start."
                    return 'start'
            if load_slot == 3:
                if load_data.save3:
                    PARTY = load_data.save3.party
                    return load_data.save3.scene_name
                else:
                    print "No save file found. Returning to start."
                    return 'start'
            if load_slot == 0:
                formatborders(
                    "Wrong slot name. Please enter 1,2, or 3. Returning to Start."
                )
                return 'start'
        elif start_option in ['3', 'help', 'Help', 'h', 'H']:
            return 'help'

        elif start_option in ['4', 'Exit', 'exit', 'q', 'Q', 'quit', 'Quit']:
            formatborders("Exiting game. Thank you for playing.")
            quit()

        else:
            print "Please select a proper option."
            return 'start'

    def check_memory_card(self):
        try:
            load_file = open('save.txt', 'r')
        except:
            load_file = open('save.txt', 'w')
        try:
            load_data = pickle.load(load_file)

        except:
            load_data = MemoryCard()

        load_file.close()
        return load_data


class Finished(Scene):
    def enter(self):
        print "You finished"


class Monster(object):

    pass


class Menu(Scene):
    def enter(self, scene_name):

        self.menu_text = """

        1. Stats
        2. Inventory
        3. Magic
        4. Equipment
        5. Save
        6. Load
        7. Exit Menu
        8. Quit Game
        9. Help
        10. Debug Menu
        """
        print self.menu_text
        choice = None

        while choice not in ["exit", "return", "7", "q", "quit"]:

            choice = raw_input("What would  you like to do: \n>")

            if choice in ['stats', 'Stats', '1']:
                self.stats(PARTY)
                print self.menu_text
            if choice in [
                    'inventory', 'Inventory', 'inv', 'Inv', '2', 'i', 'I'
            ]:
                return 'inv'
            if choice in ['magic', 'Magic', 'm', 'M', '3']:
                return 'magic'
            if choice in ['save', 'Save', '5']:
                return 'save'
            if choice in [
                    '4', 'equip'
                    'Equip', 'equipment', 'Equipment', 'e', 'E'
            ]:
                return 'equip'
            if choice in ['6', 'load', 'Load', 'l', 'L']:
                return 'load'
            if choice in ['8', 'quit', 'q', 'Q']:
                return 'finished'
            if choice in ['9', 'help', 'Help', 'h', 'H']:
                return 'mhelp'
            if choice in ['10', 'd']:
                return 'debug'

        return scene_name

    def stats(self, party):

        stat_names = [
            'name', 'job', 'lev', 'hp', 'mp', 'exp', 'mag', 'mdef', 'stre',
            'defe', 'speed'
        ]

        formatborders("STATS")
        PARTY.display_full_panel()
        raw_input("Press Enter")


class SubMenu(Scene):
    def enter(self, scene_name):
        print "You have accessed a submenu. It hasn't been initialized yet."
        return 'menu'


class InventoryMenu(SubMenu):
    def enter(self, scene_name):
        formatborders("INVENTORY")
        print "GIL : %d"
        print "-" * 80
        item_list = PARTY.inv.display_items('item')
        item_num_type = PARTY.inv.select_item(item_list)
        if item_num_type:
            PARTY.inv.use_item(item_num_type[1])
        return 'menu'


class MagicMenu(SubMenu):
    def enter(self, scene_name):
        formatborders("MAGIC")
        hero_number = 1
        hero_list = []
        for hero in PARTY.get_party_members():
            hero_list.append(hero)
            print str(hero_number) + ".",
            hero.display_spells()
            hero_number += 1
        print "Enter Hero Number of spell to use: (Press enter to return to menu)"
        hero_choice = raw_input("\n>")
        if hero_choice in ['1', '2', '3', '4']:
            if len(hero_list[int(hero_choice) - 1].spells) == 0:
                print "Hero has no spells yet. Returning to menu"
            else:
                print "Enter spell number to use:"
                hero = hero_list[int(hero_choice) - 1]
                spell_choice = raw_input("\n>")
                if spell_choice.isdigit():
                    spell_choice = int(spell_choice)
                    if (spell_choice <= len(hero.spells)) and (spell_choice >=
                                                               1):
                        spell = hero.spells[spell_choice - 1]
                        if spell.foe_target == 'foe':
                            print spell.desc
                        else:
                            print spell.desc
                            print "Select a hero to use the spell on. "
                            PARTY.display_panel_numbered()
                            target_choice = raw_input("\n>")
                            if (int(target_choice) <=
                                    4) and (int(target_choice) >= 1):
                                hero.use_spell(
                                    spell, hero_list[int(target_choice) - 1])
                            else:
                                print "Please select a hero numbered 1 through 4"
                else:
                    print "Enter a proper spell number. returning to menu."

        return 'menu'


class EquipmentMenu(SubMenu):
    def enter(self, scene_name):
        PARTY.display_equipment()
        equip_list = PARTY.inv.display_items('equip')
        PARTY.inv.select_equipment(equip_list)

        return 'menu'


class SaveMenu(SubMenu):
    def enter(self, scene_name):

        load_data = self.check_memory_card()
        load_data.display_saves()
        save_slot = raw_input("Which slot do you want to use: \n>")

        try:
            save_slot = int(save_slot)
        except:
            save_slot = 0

        if save_slot in [1, 2, 3]:
            print "Saving game......."
            if save_slot == 1:
                load_data.save1 = Save()
                load_data.save1.save_game(scene_name)

            if save_slot == 2:
                load_data.save2 = Save()
                load_data.save2.save_game(scene_name)
            if save_slot == 3:
                load_data.save3 = Save()
                load_data.save3.save_game(scene_name)

            print "Saving Complete. Writing to file......."
            save_file = open('save.txt', "w+")
            pickle.dump(load_data, save_file)
            save_file.close()
        else:
            print "Improper save entered. Returning to menu. Game NOT saved."

        return 'menu'

    def check_memory_card(self):
        try:
            load_file = open('save.txt', 'r')
        except:
            load_file = open('save.txt', 'w')
        try:
            load_data = pickle.load(load_file)

        except:
            load_data = MemoryCard()

        load_file.close()
        return load_data


class LoadMenu(SubMenu):
    global PARTY

    def enter(self, scenename):
        global PARTY
        print "Loading Save File........"
        load_data = self.check_memory_card()
        load_data.display_saves()

        load_slot = raw_input("Which file would you like to load: \n>")
        try:
            load_slot = int(load_slot)
        except:
            load_slot = 0

        if load_slot == 1:
            if load_data.save1:
                PARTY = load_data.save1.party
                return load_data.save1.scene_name
            else:
                print "No save file found. Returning to Menu."
                return 'menu'
        if load_slot == 2:
            if load_data.save2:
                PARTY = load_data.save2.party
                return load_data.save2.scene_name
            else:
                print "No save file found. Returning to Menu."
                return 'menu'
        if load_slot == 3:
            if load_data.save3:
                PARTY = load_data.save3.party
                return load_data.save3.scene_name
            else:
                print "No save file found. Returning to Menu."
                return 'menu'
        if load_slot == 0:
            formatborders(
                "Wrong slot name. Please enter 1,2, or 3. Returning to Menu.")
            return 'menu'

    def check_memory_card(self):
        try:
            load_file = open('save.txt', 'r')
        except:
            load_file = open('save.txt', 'w')
        try:
            load_data = pickle.load(load_file)

        except:
            load_data = MemoryCard()

        load_file.close()
        return load_data


class Field_0_1(Scene):

    text = """

    You have entered a field.

    1. Menu
    2. Go to town
    3. Go to field.

    """

    def enter(self):

        print self.text
        location = raw_input("Enter a number: \n>")

        if location in ['1', 'menu', 'Menu', 'm', 'M']:
            return 'menu'
        elif location in ['2', 'town', 'Town']:

            return 'town1'
        elif location in ['3', 'field', 'f', 'Field', 'F']:
            return 'field_1_1'
        else:
            return 'field_0_1'


class Field_1_1(Scene):

    text = """

    You have entered another field.

    1. Menu
    2. Go back to the field.

    """

    def enter(self):

        print self.text
        location = raw_input("Enter a number: \n>")

        if location in ['1', 'menu', 'Menu', 'm', 'M']:
            return 'menu'
        elif location in ['2', 'field', 'Field']:

            return 'field_0_1'
        else:
            return 'field_1_1'


class Town1(Scene):

    text = """

    You have entered the town of Alexandria. It is a quaint village with narrow
streets filled with villagers milling about. There is an item shop and an inn
visible from the main street.

    1. Menu
    2. Go to back to the field.
    3. Item Shop
    4. Inn
    5. Poke a villager
    %s
"""

    def enter(self):
        formatborders("ALEXANDRIA:")
        level_sum = 0
        hp_sum = 0
        for hero in PARTY.get_party_members():
            level_sum += hero.lev
        if level_sum < 5:
            print self.text % ""
        else:
            print self.text % "6. Weapon Shop"
        location = raw_input("Enter a number: \n>")

        if location in ['1', 'menu', 'Menu', 'm', 'M']:
            return 'menu'
        if location in ['2', 'field', 'Field', 'f', 'F']:
            return 'field_0_1'
        if location in ['3', 'item', 'Item']:
            return 'itemshop1'
        if location in ['4', 'inn', 'Inn']:
            return 'inn1'
        if location in ['5', 'poke', 'Villager']:
            print "-" * 80
            print "OUCH! What kind of pervert goes around poking a poor old man?"
            print "You should be ashamed of yourselves."
            print "If you had asked me nicely I would have told you that the"
            print "weapons shop only sells to parties that are level 2 and higher."
            print "\nBut because you're rude all I'm going to do is fart in your general direction"
            print "\n *BLURTTTT*"
            print "The old man is propelled forward into a wall and is knocked out."
            print "Your eyes begin burning from the smell and everone loses 1 HP."
            print "-" * 80

            if self.game_over_scene(1) == 0:
                formatborders("GAME OVER: DEATH BY OLD MAN FART")
                return 'gameover'
            if PARTY.gil < 50:
                print "You find 5 gil on the ground. It must have fallen from the old man's pocket."
                PARTY.gil += 5
            return 'town1'
        if location in ['6', 'weapon', 'Weapon', 'w', 'W']:
            return 'weaponshop1'
        else:
            return 'town1'


class ItemShop1(Scene):
    item_text = """

You walk into through the shop doors to meet the shopkeeper sitting on a stool.
He blows a cloud of strange smelling smoke in your face and says "Welcome to the
pot shop.... I mean potion shop. You guys aren't cops right?"

You shake your head no and he proceeds to show you his wares.

Enter an option.

    1. Potion           20 gil
    2. Ether           100 gil
    3. Phoenix Down    500 gil
    4. Return to town

    """

    def enter(self):
        formatborders("POT SHOP: ALEXNADRIA")
        print self.item_text
        option = raw_input(">")

        try:

            if option in ['1', 'potion', 'Potion']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 25:
                    PARTY.inv.add_item(potion, int(amount))
                    PARTY.gil -= int(amount) * 25
                    print "Bought x%d potions for %d Gil" % (int(amount),
                                                             int(amount) * 25)
                    print '-' * 80
                else:
                    print "Not enough minerals."
                return 'itemshop1'
            elif option in ['2', 'ether', 'Ether']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 100:
                    PARTY.inv.add_item(ether, int(amount))
                    PARTY.gil -= int(amount) * 100
                    print "Bought x%d ethers for %d Gil" % (int(amount),
                                                            int(amount) * 100)
                    print '-' * 80
                else:
                    print "Not enough vespene gas."
                return 'itemshop1'
            elif option in ['3', 'phoenix', 'down', 'Phoenix', 'Down']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 500:
                    PARTY.inv.add_item(phoenixdown, int(amount))
                    PARTY.gil -= int(amount) * 500
                    print "Bought x%d phoenix downss for %d Gil" % (
                        int(amount), int(amount) * 500)
                    print '-' * 80
                else:
                    print "Build more supply depots."
                return 'itemshop1'
            else:
                print "Proper option not selected."
        except:
            print "Please enter an integer for the item amount."
        print "Returning to town. "

        return 'town1'


class Inn1(Scene):

    inn_text = """

As you walk into the inn the innkeeper offers you a room to stay for the night.
He also offers you licentious 'favors' with a wink.

    It costs 100 gil to rest.
    It costs 100,000,000 gil for a happy massage.

    Choose an action:

    1. Rest
    2. Massage
    3. Return to Town

    """

    def enter(self):
        formatborders("INN: ALEXANDRIA")
        print self.inn_text
        option = raw_input(">")
        if option in ['1', 'rest', 'Rest', 'R', 'r']:
            if PARTY.gil >= 100:
                self.restore_party()

                PARTY.gil -= 100
            else:
                print "The inkeeper rages at you trying to stay without enough money."
                print "You flee back to town as he picks up a shotgun an aims it at you."
                return 'town1'
        if option in ['2', 'massage', 'Massage', 'm', 'M']:
            if PARTY.gil >= 100000000:
                print "OH BABY."
                PARTY.gil -= 100000000
            else:
                print "You aren't yet ready for such things."
                return 'town1'

        return 'town1'


class WeaponShop1(Scene):

    item_text = """

You walk into through the shop doors to meet the shopkeeper sitting on a stool.
He blows a cloud of strange smelling smoke in your face and says "Welcome to the
pot shop.... I mean potion shop. You guys aren't cops right?"

You shake your head no and he proceeds to show you his wares.

Enter an option.

    1. Iron Sword          400 gil
    2. Iron Chest          300 gil
    3. Iron Helm           200 gil
    4. Steel Wand          350 gil
    5. Cloth Chest         250 gil
    6. Cloth Helm          500 gil
    7. Return to town

    """

    def enter(self):
        formatborders("WEAPON SHOP: ALEXNADRIA")
        print self.item_text
        option = raw_input(">")

        try:

            if option in ['1', 'potion', 'Potion']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 25:
                    PARTY.inv.add_item(potion, int(amount))
                    PARTY.gil -= int(amount) * 25
                    print "Bought x%d potions for %d Gil" % (int(amount),
                                                             int(amount) * 25)
                    print '-' * 80
                else:
                    print "Not enough minerals."
                return 'itemshop1'
            elif option in ['2', 'ether', 'Ether']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 100:
                    PARTY.inv.add_item(ether, int(amount))
                    PARTY.gil -= int(amount) * 100
                    print "Bought x%d ethers for %d Gil" % (int(amount),
                                                            int(amount) * 100)
                    print '-' * 80
                else:
                    print "Not enough vespene gas."
                return 'itemshop1'
            elif option in ['3', 'phoenix', 'down', 'Phoenix', 'Down']:
                amount = raw_input("Enter a quantity to buy: \n>")
                if PARTY.gil >= int(amount) * 500:
                    PARTY.inv.add_item(phoenixdown, int(amount))
                    PARTY.gil -= int(amount) * 500
                    print "Bought x%d phoenix downss for %d Gil" % (
                        int(amount), int(amount) * 500)
                    print '-' * 80
                else:
                    print "Build more supply depots."
                return 'itemshop1'
            else:
                print "Proper option not selected."
        except:
            print "Please enter an integer for the item amount."
        print "Returning to town. "

        return 'town1'


class Battle(Scene):

    calmlands = [Slime]
    area_monsters = {'field_0_1': calmlands, 'field_1_1': calmlands}

    monster_text = [
        "A random %s has appeared!",
        "A %s has suddently manifested. It looks angry.",
        "A %s looks at you and  you wet your pants. ",
        "You hear battle music start to play in your head as a %s charges you.",
        "POP! A %s begins twerking menacingly in your direction. ",
    ]

    def enter(self, current_area):
        print "Entering a battle from %s. " % current_area
        mobs = self.generate_monsters(current_area)
        Hero_Classes = [Warrior, BlackMage, WhiteMage, Theif]
        formatborders("BATTLE")

        for monster in mobs:
            print self.monster_text[randint(
                0,
                len(self.monster_text) - 1)] % monster.__class__.__name__

        time_table, name_table = self.generate_turn_table(mobs)

        PARTY.display_panel()
        loot = []
        battle_exp = 0
        gil_reward = 0
        alive_table = self.generate_alive_table()

        self.display_panel(mobs)
        battle_logic = 0
        k = 0
        while battle_logic == 0:

            index = 0
            for time in time_table:

                if time_table[index] == 0:
                    current = name_table[index]
                    if current.hp > 0 and current.__class__ in Hero_Classes:
                        alive_table[index] = 1
                    if current.hp > 0:
                        damage, target = current.act(name_table)
                        name_table[target].hp -= damage
                        time_table[index] = current.time

                    if name_table[target].hp <= 0 and (
                            name_table[target].__class__ in Hero_Classes) and (
                                current.__class__ not in Hero_Classes):
                        name_table[target].hp = 0
                        alive_table[target] = 0

                        print "%s has been slain." % name_table[target].name
                        if 1 not in alive_table:
                            battle_logic = 2
                            formatborders("GAME OVER")
                            return 'gameover'

                    if name_table[target].__class__ not in Hero_Classes:
                        if name_table[target].hp <= 0:
                            print "%s has been slain." % name_table[
                                target].__class__.__name__
                            loot.extend(name_table[target].rewards())
                            battle_exp += name_table[target].exp
                            gil_reward += randint(1, name_table[target].lev
                                                  **1.5)
                            del name_table[target]
                            del time_table[target]
                            del mobs[target - 4]
                    if current.hp > 0 and current.__class__ in Hero_Classes:
                        PARTY.display_panel()
                    if mobs and (current.__class__ in Hero_Classes
                                 ) and current.hp > 0:
                        self.display_panel(mobs)
                    if len(mobs) == 0:
                        if loot:
                            self.display_loot(loot)
                        print "Recieved %s exp" % battle_exp
                        self.award_exp(battle_exp)
                        PARTY.gil += gil_reward
                        print "Battle over"
                        battle_logic = 1
                        break
                time_table[index] -= 1
                index += 1

    def generate_monsters(self, current_area):

        monster_pool = self.area_monsters[current_area]
        monster_amount = randint(1, 3)
        monsters = []
        level = PARTY.return_avg_levels()
        for n in range(monster_amount):
            monster_choices = len(monster_pool)
            monster_choice = randint(0, monster_choices - 1)
            monsters.append(monster_pool[monster_choice](level))

        return monsters

    def generate_turn_table(self, monsters):

        turn_table = []
        turn_names = []
        for hero in PARTY.get_party_members():
            turn_table.append(hero.time)
            turn_names.append(hero)
        for monster in monsters:
            turn_table.append(monster.time)
            turn_names.append(monster)

        # print turn_table
        # print turn_names
        return turn_table, turn_names

    def display_hp(self, monsters):
        display_format = "{:<10}" + ("{:^10}" * len(monsters))

        monster_hp = ["HP:"]
        for monster in monsters:
            monster_hp.append("%d/%d" % (monster.hp, monster.maxhp))

        print display_format.format(*monster_hp)

    def display_monster_names(self, monsters):
        display_format = "{:<10}" + ("{:^10}" * len(monsters))
        monster_names = ['Monster:']
        for monster in monsters:
            monster_names.append(monster.__class__.__name__)

        print display_format.format(*monster_names)

    def display_panel(self, monsters):

        self.display_monster_names(monsters)
        self.display_hp(monsters)
        print "-" * 80

    def display_loot(self, item_list):
        text_list = []
        loot_dict = {}
        for item in item_list:
            text_list.append(item.name)
            PARTY.inv.add_item(item, 1)

        for item in text_list:
            try:
                loot_dict[item] += 1
            except:
                loot_dict[item] = 1
        item_text = ""
        for k, v in loot_dict.items():
            item_text += "%s x %d " % (k, v)
        display_text = "Obtained: %s" % item_text
        battleborders(display_text)

    def award_exp(self, battle_exp):
        for hero in PARTY.get_party_members():
            if hero.hp > 0:
                hero.exp -= battle_exp
                if hero.exp <= 0:
                    leftover_exp = -hero.exp
                    hero.level_up(leftover_exp)

    def generate_alive_table(self):
        alive_table = []
        for hero in PARTY.get_party_members():
            if hero.hp > 0:
                alive_table.append(1)
            else:
                alive_table.append(0)
        return alive_table


class Help(Scene):

    help_text = """

    Python Fantasy is a text based role playing game. You navagate through
screens by using the number of the command you wish to select, or typing out the
option itself.

    For example look at the following menu:

    1. Menu
    2. Inventory
    3. Stats
    4. Exit

    To access the menu you can enter 1, Menu, or menu. To access the Inventory
you can enter 2, Inventory, inv, or Inv. To exit you can enter 4, Exit, exit, or
anything other than the options given (q for instance).

    Entering q on most selection options will return you to the previous menu.
To exit the game, enter the menu and select option 8.

    Press Enter now to return.

    """

    def enter(self, scenename):

        formatborders("HELP")
        print self.help_text

        go_back = raw_input()

        return scenename


class MHelp(Scene):

    help_text = """

    Python Fantasy is a text based role playing game. You navagate through screens
by using the number of the command you wish to select, or typing out the option
itself. The game mechanics are lovingly inspired by Final Fantasy by Squaresoft.

    For example look at the following menu:

    1. Menu
    2. Inventory
    3. Stats
    4. Exit

    To access the menu you can enter 1, Menu, or menu. To access the Inventory
you can enter 2, Inventory, inv, or Inv. To exit you can enter 4, Exit, exit, or
anything other than the options given (q for instance).


    Entering q on most selection options will return you to the previous menu.

    Press Enter now to return. Enter 1 to view Stat help.

    """
    stat_text = """
    JOB  - The class of the hero
    LEV  - The level of the hero, the hero grows stronger with higher levels
    HP   - Health Points, how much health the hero has. When HP goes to 0 the
           hero dies and can only be revived by using a phoenix doewn or by
           resting at an inn. Can be restored with items such as a potion.
    MP   - Mana Points, how much energy the hero has to cast spells. Can be
           replenished with items such as ethers.
    EXP  - Experience points remaining to level up.
    MAG  - Magic, magical attack effectiveness
    MDEF - Magical Defence, ability to resist magical attacks
    STRE - Strength, physical attack effectiveness
    DEFE - Defence, ability to resist physical attacks
    SPEED- How soon a hero's turn will come in battle.

    Press Enter to return to menu.

    """

    def enter(self, scenename):

        formatborders("HELP")
        print self.help_text

        go_back = raw_input(">")

        if go_back in ['1', 'stat', 's', 'S']:
            print self.stat_text
            go = raw_input()

        return 'menu'


class Debug(Scene):
    def enter(self, scenename):

        print "Debug menu"

        print """
        1. print MP stat for hero 1
        2. print dir(hero1)
        3. print party stats
        4. add 90 exp to party
        5. kill hero 1 add phoenix down to inventory
        6. Enter command
        7. Set party hp to 1
        8. Enter battle from field 1
        9. kill all heros but hero 4
        10. Levelup heros to 50
        11. Add Cure and fire to spellbook

        """

        option = raw_input("\n>")

        if option == '1':
            PARTY.display_stat(PARTY.hero1.mp)
        if option == '2':
            print dir(PARTY.hero1)
        if option == '3':
            PARTY.display_full_panel()
        if option == '4':
            for hero in PARTY.get_party_members():
                hero.exp -= 90
        if option == '5':
            PARTY.hero1.hp = 0
            PARTY.inv.add_item(phoenixdown, 1)
        if option == '6':
            command = raw_input("Enter a Command\n>")
            exec (command)
        if option == '7':
            for hero in PARTY.get_party_members():
                hero.hp = 1
        if option == '8':
            Battle().enter('field_1_1')
        if option == '9':
            for hero in PARTY.get_party_members()[:3]:
                hero.hp = 0
            PARTY.hero4.hp = 1

        if option == '10':
            for hero in PARTY.get_party_members():
                for x in range(50):
                    hero.level_up(0)
        if option == '11':
            PARTY.hero2.spells.append(cure)
            PARTY.hero3.spells.append(fire)

        return 'menu'


class GameOver(Scene):

    game_over_text = """

    Game over.
    You were either unprepared or extrememly unlucky.
    Or both.

    You now have 3 options.
    1. Load Game
    2. Main Menu
    3. Stop playing.

    """

    def enter(self):
        print self.game_over_text
        option = raw_input(">")

        if option in ['1', 'l', 'L', 'load', 'Load']:
            return 'load'
        if option in ['2', 'm', 'M', 'main', 'Main', 'menu', 'Menu']:
            return 'start'
        if option in ['3', 'stop', 's']:
            print "I <3 pi. "
            quit()
        if option == None:
            "Please select an option."
            self.enter()


class Map(object):

    scenes = {
        'open': Opening(),
        'finished': Finished(),
        'menu': Menu(),
        'inv': InventoryMenu(),
        'magic': MagicMenu(),
        'save': SaveMenu(),
        'start': GameStart(),
        'help': Help(),
        'mhelp': MHelp(),
        'equip': EquipmentMenu(),
        'load': LoadMenu(),
        'town1': Town1(),
        'itemshop1': ItemShop1(),
        'inn1': Inn1(),
        'weaponshop1': WeaponShop1(),
        'field_0_1': Field_0_1(),
        'field_1_1': Field_1_1(),
        'debug': Debug(),
        'battle': Battle(),
        'gameover': GameOver()
    }

    towns = ['town1', 'itemshop1', 'inn1', 'weaponshop1']

    invscenes = {v: k for k, v in scenes.items()}

    def __init__(self, start_scene):
        self.start_scene = start_scene

    def next_scene(self, scene_name):
        val = Map.scenes.get(scene_name)
        return val

    def name_of_scene(self, scene):
        val = Map.invscenes.get(scene)
        return val

    def opening_scene(self):
        return self.next_scene(self.start_scene)


class Save(object):
    def save_game(self, scene_name):
        self.scene_name = scene_name
        self.party = PARTY


class MemoryCard(object):
    def __init__(self):
        self.save1 = None
        self.save2 = None
        self.save3 = None

    def display_saves(self):
        formatborders("SAVE SLOTS")
        try:
            print "SLOT 1 : %s" % Map.scenes[self.save1.
                                             scene_name].__class__.__name__
            for hero in self.save1.party.get_party_members():
                print hero.name.upper().ljust(10), "LVL: ", str(
                    hero.lev).ljust(2),
        except:
            print "SLOT 1 : EMPTY"
        print "\n"
        try:
            print "SLOT 2 : %s" % Map.scenes[self.save2.
                                             scene_name].__class__.__name__
            for hero in self.save2.party.get_party_members():
                print hero.name.upper().ljust(10), "LVL: ", str(
                    hero.lev).ljust(2),
        except:
            print "SLOT 2 : EMPTY"
        print "\n"
        try:
            print "SLOT 3 : %s" % Map.scenes[self.save3.
                                             scene_name].__class__.__name__
            for hero in self.save3.party.get_party_members():
                print hero.name.upper().ljust(10), "LVL: ", str(
                    hero.lev).ljust(2),
        except:
            print "SLOT 3 : EMPTY"
        print "\n"

        formatborders(
            "Enter the slot number or press enter to return to previous screen"
        )
        print "\n"


def formatborders(textinput):
    print '=' * 80
    print textinput
    print '=' * 80


def battleborders(textinput):
    print '-' * 80
    print textinput
    print '-' * 80


def nl():
    print "\n"


PARTY = None
Hero_Classes = [Warrior, BlackMage, WhiteMage, Theif]
a_map = Map('start')

a_game = Engine(a_map)
a_game.play()


{% endhighlight %}
