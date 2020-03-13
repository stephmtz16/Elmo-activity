# Elmo-activity
color piano with sound

#include <allegro5/allegro.h>
#include<allegro5/allegro_primitives.h>
#include<allegro5/allegro_image.h>
#include<iostream>
using namespace std;
#include<Windows.h> //because I was too lazy to download wav files for each scale note :/
//what are constants and why is it seen as good practice to use them?
const float FPS = 60;
const int SCREEN_W = 1000;
const int SCREEN_H = 600;
const int REDX = 100; //do more of thses
const int ORANGEX = 200;
const int YELLOWX = 300;
const int LIMEX = 400;
const int GREENX = 500;
const int TEALX = 600;
const int BLUEX = 700;
const int PURPLEX = 800;
const int RECTY = 450;
const int RECTW = 100;
const int RECTH = 150;


enum COLORS { NOTHING, RED, ORANGE, YELLOW, LIME, GREEN, TEAL, BLUE, PURPLE }; //what number is attached to each word?
int checkColor(float x, float y);
int main()
{
	al_init();
	al_init_primitives_addon();
	al_init_image_addon();
	al_install_mouse();
	ALLEGRO_DISPLAY* display = al_create_display(SCREEN_W, SCREEN_H);
	ALLEGRO_EVENT_QUEUE* event_queue = al_create_event_queue();
	ALLEGRO_TIMER* timer = al_create_timer(1.0 / FPS);
	ALLEGRO_BITMAP* street = al_load_bitmap("street.jpg");
	bool redraw = true;
	float xPos = 400;
	float yPos = 400;
	int color[] = { false, false, false, false,false, false,false,false,false }; //why do I have 4 values in there but just 3 colors?
	al_register_event_source(event_queue, al_get_display_event_source(display));
	al_register_event_source(event_queue, al_get_timer_event_source(timer));
	al_register_event_source(event_queue, al_get_mouse_event_source());
	al_start_timer(timer);
	while (1)
	{
		ALLEGRO_EVENT ev;
		al_wait_for_event(event_queue, &ev);
		if (ev.type == ALLEGRO_EVENT_TIMER) {
			if (color[RED])
				Beep(262, 200); //Never ever do this again. In fact, never tell anyone I did this either.
			if (color[ORANGE])
				Beep(294, 200);
			if (color[YELLOW])
				Beep(330, 200);
			if (color[LIME])
				Beep(349, 200);
			if (color[GREEN])
				Beep(392, 200);
			if (color[TEAL])
				Beep(440, 200);
			if (color[BLUE])
				Beep(494, 200);
			if (color[PURPLE])
				Beep(523, 200);
			//what does this do?
			for (int i = 0; i < 9; i++)//runs through
				color[i] = false;//array
			redraw = true;
		}
		else if (ev.type == ALLEGRO_EVENT_DISPLAY_CLOSE) {
			break;
		}
		else if (ev.type == ALLEGRO_EVENT_MOUSE_AXES ||
			ev.type == ALLEGRO_EVENT_MOUSE_ENTER_DISPLAY) {
			xPos = ev.mouse.x;
			yPos = ev.mouse.y;
		}
		else if (ev.type == ALLEGRO_EVENT_MOUSE_BUTTON_UP) {
			//this is redundant; the array is already reset in the timer section
			//if we were using something better than beep, this would be good to have... can you explain why?
			for (int i = 0; i < 4; i++)
				color[i] = false;
		}
		else if (ev.type == ALLEGRO_EVENT_MOUSE_BUTTON_DOWN) {
			if (checkColor(xPos, yPos) == RED)
				color[RED] = true;
			if (checkColor(xPos, yPos) == ORANGE)
				color[ORANGE] = true;
			if (checkColor(xPos, yPos) == YELLOW)
				color[YELLOW] = true;
			if (checkColor(xPos, yPos) == LIME)
				color[LIME] = true;
			if (checkColor(xPos, yPos) == GREEN)
				color[GREEN] = true;
			if (checkColor(xPos, yPos) == TEAL)
				color[TEAL] = true;
			if (checkColor(xPos, yPos) == BLUE)
				color[BLUE] = true;
			if (checkColor(xPos, yPos) == PURPLE)
				color[PURPLE] = true;
		}
		if (redraw && al_is_event_queue_empty(event_queue)) {
			redraw = false;
			al_clear_to_color(al_map_rgb(0, 0, 0));
			//background image
			al_draw_bitmap(street, 0, 0, NULL);
			//draw color boxes
			al_draw_filled_rectangle(REDX, RECTY, REDX + RECTW, RECTY + RECTH, al_map_rgb(250, 0, 0)); //red
			al_draw_filled_rectangle(ORANGEX, RECTY, ORANGEX + RECTW, RECTY + RECTH, al_map_rgb(250, 100, 0)); //orange
			al_draw_filled_rectangle(YELLOWX, RECTY, YELLOWX + RECTW, RECTY + RECTH, al_map_rgb(250, 250, 0)); //yellow
			al_draw_filled_rectangle(LIMEX, RECTY, LIMEX + RECTW, RECTY + RECTH, al_map_rgb(51, 255, 51)); //lime
			al_draw_filled_rectangle(GREENX, RECTY, GREENX + RECTW, RECTY + RECTH, al_map_rgb(0, 153, 0)); //green
			al_draw_filled_rectangle(TEALX, RECTY, TEALX + RECTW, RECTY + RECTH, al_map_rgb(0, 255, 255)); //teal
			al_draw_filled_rectangle(BLUEX, RECTY, BLUEX + RECTW, RECTY + RECTH, al_map_rgb(0, 0, 255)); //blue
			al_draw_filled_rectangle(PURPLEX, RECTY, PURPLEX + RECTW, RECTY + RECTH, al_map_rgb(178, 102, 255)); //purple
			//draw pointer
			al_draw_filled_circle(xPos, yPos, 10, al_map_rgb(200, 200, 0));
			al_draw_circle(xPos, yPos, 10, al_map_rgb(0, 0, 0), 5);
			al_flip_display();
		}
	}
	al_destroy_timer(timer);
	al_destroy_display(display);
	al_destroy_event_queue(event_queue);
	return 0;
}
int checkColor(float x, float y) {
	//how is this similar to the bounding box collision of pong/breakout?
	//how is it different?
	if (x >= 100 && x <= 200 && y >= 450 && y <= 600) { //check if on red
		cout << "red" << endl; //optional- put in for debugging purposes
		return RED;
	}
	else if (x >= 200 && x <= 300 && y >= 450 && y <= 600) {//check if on orange
		cout << "orange" << endl;
		return ORANGE;
	}
	else if (x >= 300 && x <= 400 && y >= 450 && y <= 600) { //check if on yellow
		cout << "yellow" << endl;
		return YELLOW;
	}
	else if (x >= 400 && x <= 500 && y >= 450 && y <= 600) { //check if on lime
		cout << "lime" << endl;
		return LIME;
	}
	else if (x >= 500 && x <= 600 && y >= 450 && y <= 600) { //check if on green
		cout << "green" << endl;
		return GREEN;
	}
	else if (x >= 600 && x <= 700 && y >= 450 && y <= 600) { //check if on teal
		cout << "teal" << endl;
		return TEAL;
	}
	else if (x >= 700 && x <= 800 && y >= 450 && y <= 600) { //check if on blue
		cout << "blue" << endl;
		return BLUE;
	}
	else if (x >= 800 && x <= 900 && y >= 450 && y <= 600) { //check if on purple
		cout << "purple" << endl;
		return PURPLE;
	}
	else
		return NOTHING;
}
