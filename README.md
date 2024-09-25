# gifs.service.ts      SERVICIO PARA BUSQUEDA
import { Injectable } from '@angular/core';

@Injectable({providedIn: 'root'})
export class GifsService {

    private _tagHistory: string[]= []


    constructor() { }
    

    get tagHistory(){
        return [...this._tagHistory]
    }

    searchTag (tag:string):void{
        this._tagHistory.unshift(tag)
    }

}


****************************************

**#search-box.component.ts**
import { Component, ElementRef, OnInit, ViewChild} from '@angular/core';
import { GifsService } from '../../services/gifs.service';

@Component({
    selector: 'gifs-search-box',
    template: `
        <div class="flex flex-col p-2 m-2 w-full">
        <h5>Buscar: </h5>
        <input type="text"
            class="border-2 border-gray-800 p-1 rounded-md flex-grow"
            placeholder="Buscar Gifs..." 
            (keyup.enter)="searchTag()"
            #txtTagInput
        >
        </div>

    `
})

export class SearchBoxComponent implements OnInit {
    constructor( private GifsService:GifsService ) { }

    ngOnInit() { }

    @ViewChild('txtTagInput')
    public tagInput!: ElementRef<HTMLInputElement>

    searchTag(){
        const newTag = this.tagInput.nativeElement.value
        this.GifsService.searchTag(newTag)
        console.log(newTag)
        this.tagInput.nativeElement.value = ''
    }

}
